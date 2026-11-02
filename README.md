# econ3916-lab12-ols-prediction
# OLS as a Line of Best Fit: Estimation versus Prediction

## Objective
I used the California Housing data to show that the same least-squares fit does two different jobs, estimating a coefficient and predicting an outcome, and that each job needs its own scorecard.

## Methodology
- Loaded California Housing from scikit-learn: 20,640 districts from the 1990 census, with median house value (in $100,000s) as the outcome.
- Fitted median house value on median income three ways: the closed-form slope formula, statsmodels, and scikit-learn. I checked that all three gave the same intercept and slope.
- Added 0, 1, 5, 20 and 100 columns of random noise next to median income and recorded R-squared and adjusted R-squared each time.
- Plotted the fitted line and the residuals against the fitted values to look for problems that R-squared does not show.
- Split the data 80/20 into training and test rows before fitting anything, fitted an eight-feature model on the training rows, and compared RMSE on the training rows with RMSE on the test rows.
- Used one specification (median income and rooms per household) for two jobs: estimating the coefficient on rooms with a 95% confidence interval, and measuring test RMSE.
- Wrote a function, holdout_rmse(), that splits the data the same way, fits on the training rows and returns the test RMSE for any list of predictors. I used it to score median income on its own.

## Key Findings
- All three methods gave the same line: intercept 0.4509 and slope 0.4179. A $10,000 rise in median income goes with about a $41,800 rise in median house value.
- R-squared rose from 0.4734 with no noise to 0.4758 with 100 noise columns, even though the noise has nothing to do with house value. Adjusted R-squared went from 0.4734 to 0.4732. It rose slightly at 5 columns before falling, so it is not a safe way to choose models either.
- The residual plot showed two problems: a diagonal edge caused by house values being capped at 5.0 ($500,001), and residual spread that changes with the fitted value.
- The eight-feature model had a train RMSE of 0.7197 and a test RMSE of 0.7456, a typical error of about $74,600 per district.
- The AveRooms coefficient, holding income fixed, was -0.0381, with a 95% CI of [-0.0430, -0.0333]. That is a precise estimate, but the same specification had a test RMSE of 0.8379. A precise coefficient does not mean good predictions.
- With median income alone, the test RMSE was [YOUR VALUE], about $[GAP] more error per district than the eight-feature model. Because both were scored on the same unseen test rows, this is a fair comparison in a way that in-sample R-squared is not.
- (Challenge) [YOUR VALUE] districts sit at the 5.0 ceiling. Dropping them changed the median income coefficient from [YOUR VALUE] to [YOUR VALUE], and test RMSE on the uncensored test rows went from [YOUR VALUE] to [YOUR VALUE]. The model without them only describes districts worth under $500,000.
