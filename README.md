# Price analysis

## Objective

Use provided time series data to build a statistical model specification to explore relationship between Price and the other variables in the dataset. 

## Analysis overview
   1. input data
      * initial look at dataset: number of observations, number of variables, create _period_ column to index dataset
   2. exam data
      * check missing values per variable and drop the observations with missing values
      * visualize time series of price and other variables
   3. check/test for stationarity
      * check 4-window rolling mean and rolling standard deviation of each time series data and plot against with original time series
      * conduct Augmented Dickey-Fuller test for each time series data to see if it is  stationary
   4. stationarize time series variables
      * after above step, these variables are found to be non-stationary:
          ```'Supply', 'Libor', 'USD', 'IP_World', 'IP_AdvEcon', 'IP_US', 'IP_Europe', 'IP_EmergingEcon', 'IP_EMAsia', 'CommodityIndex',               'Dx_Variable', 'XCommodityImports', 'XCommodityExports1', 'XCommodityExports2', 'XCntry_IndustrialOutput'```
      * for these non-stationary variables, these steps are taken to make them stationary
        * eliminating trend by taking log value of each time series
        * smoothing  data by substracting 4-window moving average from the logged value
        * run Augmented Dickey-Fuller test again on the post-processed time series to find still non-stationary time series: 
        ```'Supply', 'XCommodityExports1', 'XCntry_IndustrialOutput'```
        * further stationarize the remaining variables by decomposing the logged time series into: trend, seasonal, and residual; take the           residual as the further processed time series and conduct Augmented Dickey-Fuller test again;
        * after the above steps, all time series are stationary
       * stationarized variable names will have suffix '_st'
       * final review time-series after checking and making stationarity
     5. finding relationship between price and predictors
        * correlation matrix is generated
          * top correlated predictor pairs are printed
          * top correlated variable with Price are printed
        * modeling
          * step 1: Check for VIF and remove predictor with high VIF; remove top 3 high VIF features: `IP_World_st`,`IP_AdvEcon_st`,`IP_EmergingEcon_st`
          * step 2: normalize X to reduce multicollinearity and run initial model
          * step 3: check DFBETAS to see if any influential obs need to be removed 
            * consider DBETAS in absolute value greater than  2/sqrt(N)  to be influential observations
          * step 4: re-fit model after removing influential obs
          * model results summary: 
            * Adjusted R square: 0.8166
            * Predictors that show **POSTIVE** impact on Price with **strong** statistical significance (p-value < 0.05): 
              * `IP_Japan`, `XCommodityExports1_st`
            * Predictors that show **NEGATIVE** impact on Price with **strong** statistical significance (p-value < 0.05):  
              * `IP_US_st`,`USD_st`, `IP_Europe_st`
            * Predictors that show **POSITIVE** impact on Price with **less** statistical significance (p-value between 0.05 and 0.5): 
              * `CommodityIndex_st`,`XCntry_IndustrialOutput_st`, `IP_EMAsia_st`
            * Predictors that show **NEGATIVE** impact on Price with **less** statistical significance (p-value between 0.05 and 0.5):
              * `Supply_st`
            * Predictors that do **NOT** show statistical significance (p-value > 0.5):
              * `Libor_st`,`XCommodityExports2_st`,`XCommodityImports_st`,`Dx_Variable_st`

## Directory
   * `price_analysis.ipynb`: the jupyter notebook contains the entire analysis workflow with python code and findings
   
   
