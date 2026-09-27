02_exercises
================
Sglatt
2026-09-26

Exercises Book of OHDSI (Observational Health Data Sciences and
Informatics) (link: <https://ohdsi.github.io/TheBookOfOhdsi/>)

Most HADES packages need to interact with a database. HADES
`databaseConnector` makes these connections

`SqlRender` + `DatabaseConnector` provide a way to query data in the CDM

The `Eunomia` package provides a simulated dataset in the CDM that will
run inside the local R session

Importing a synthetic dataset from Eunomia via these packages and doing
some exercises

Note: the exercises have a different API then the latest version I
installed, so there’s troubleshooting sprinkled in here

# Data and packages

``` r
# Data and packages
options(repos = c(
  CRAN = "https://cran.rstudio.com"
))

if (!require("Eunomia")) {
  install.packages("Eunomia")
  library(Eunomia)
}
```

    ## Loading required package: Eunomia

    ## Warning: package 'Eunomia' was built under R version 4.4.3

``` r
install.packages(
  c("dbplyr", "DatabaseConnector"),
  type = "binary"
)
```

    ## Installing packages into 'C:/Users/Sofel/AppData/Local/R/win-library/4.4'
    ## (as 'lib' is unspecified)

    ## 
    ##   There are binary versions available (and will be installed) but the
    ##   source versions are later:
    ##                   binary source
    ## dbplyr             2.5.2  2.6.0
    ## DatabaseConnector  7.1.0  7.2.0
    ## 
    ## package 'dbplyr' successfully unpacked and MD5 sums checked
    ## package 'DatabaseConnector' successfully unpacked and MD5 sums checked

    ## Warning: cannot remove prior installation of package 'DatabaseConnector'

    ## Warning: restored 'DatabaseConnector'

    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\Sofel\AppData\Local\Temp\RtmpMRGkrw\downloaded_packages

``` r
library(DatabaseConnector)
```

    ## Warning: package 'DatabaseConnector' was built under R version 4.4.3

``` r
if (!require("SqlRender")) {
  install.packages("SqlRender")
  library(SqlRender)
}
```

    ## Loading required package: SqlRender

``` r
# Get the connection details
connectionDetails <- Eunomia::getEunomiaConnectionDetails()
```

    ## attempting to download GiBleed

    ## attempting to extract and load: C:\Users\Sofel\AppData\Local\Temp\RtmpMRGkrw/GiBleed_5.3.zip to: C:\Users\Sofel\AppData\Local\Temp\RtmpMRGkrw/GiBleed_5.3.sqlite

``` r
connectionDetails
```

    ## $dbms
    ## [1] "sqlite"
    ## 
    ## $extraSettings
    ## NULL
    ## 
    ## $oracleDriver
    ## [1] "thin"
    ## 
    ## $pathToDriver
    ## [1] ""
    ## 
    ## $user
    ## function () 
    ## rlang::eval_tidy(userExpression)
    ## <bytecode: 0x0000022f47f39000>
    ## <environment: 0x0000022f47f2aba0>
    ## 
    ## $password
    ## function () 
    ## rlang::eval_tidy(passWordExpression)
    ## <bytecode: 0x0000022f47f39460>
    ## <environment: 0x0000022f47f2aba0>
    ## 
    ## $server
    ## function () 
    ## rlang::eval_tidy(serverExpression)
    ## <bytecode: 0x0000022f47f39a48>
    ## <environment: 0x0000022f47f2aba0>
    ## 
    ## $port
    ## function () 
    ## rlang::eval_tidy(portExpression)
    ## <bytecode: 0x0000022f47f39e70>
    ## <environment: 0x0000022f47f2aba0>
    ## 
    ## $connectionString
    ## function () 
    ## rlang::eval_tidy(csExpression)
    ## <bytecode: 0x0000022f47f2a388>
    ## <environment: 0x0000022f47f2aba0>
    ## 
    ## attr(,"class")
    ## [1] "ConnectionDetails"        "DefaultConnectionDetails"

``` r
# Connect using the SQLite driver
connection <- connect(connectionDetails)
```

    ## Connecting using SQLite driver

``` r
# Create the cohorts used in Chapter 10
Eunomia::createCohorts(connectionDetails)
```

    ## Cohorts created in table main.cohort

    ##   cohortId       name
    ## 1        1  Celecoxib
    ## 2        2 Diclofenac
    ## 3        3    GiBleed
    ## 4        4     NSAIDs
    ##                                                                                        description
    ## 1    A simplified cohort definition for new users of celecoxib, designed specifically for Eunomia.
    ## 2    A simplified cohort definition for new users ofdiclofenac, designed specifically for Eunomia.
    ## 3 A simplified cohort definition for gastrointestinal bleeding, designed specifically for Eunomia.
    ## 4       A simplified cohort definition for new users of NSAIDs, designed specifically for Eunomia.
    ##   count
    ## 1  1844
    ## 2   850
    ## 3   479
    ## 4  2694

``` r
# Chapter 10
library(CohortMethod)
```

    ## Warning: package 'CohortMethod' was built under R version 4.4.3

    ## Loading required package: Cyclops

    ## Warning: package 'Cyclops' was built under R version 4.4.3

    ## Loading required package: FeatureExtraction

    ## Loading required package: Andromeda

    ## Loading required package: dplyr

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# Chapter 11
install.packages(
  "FeatureExtraction",
  repos = c(
    "https://ohdsi.github.io/drat",
    "https://cran.rstudio.com"
  )
)
```

    ## Warning: package 'FeatureExtraction' is in use and will not be installed

``` r
library(FeatureExtraction)

# Chapter 13
install.packages(
  "PatientLevelPrediction",
  repos = c(
    "https://ohdsi.github.io/drat",
    "https://cran.rstudio.com"
  )
)
```

    ## Installing package into 'C:/Users/Sofel/AppData/Local/R/win-library/4.4'
    ## (as 'lib' is unspecified)

    ## Warning: unable to access index for repository https://ohdsi.github.io/drat/bin/windows/contrib/4.4:
    ##   cannot open URL 'https://ohdsi.github.io/drat/bin/windows/contrib/4.4/PACKAGES'

    ## 
    ##   There is a binary version available but the source version is later:
    ##                        binary source needs_compilation
    ## PatientLevelPrediction  6.6.0  6.7.0             FALSE

    ## installing the source package 'PatientLevelPrediction'

``` r
library(PatientLevelPrediction)
```

    ## 
    ## Attaching package: 'PatientLevelPrediction'

    ## The following objects are masked from 'package:CohortMethod':
    ## 
    ##     createStudyPopulation, migrateDataModel

# Folder for any output

``` r
if (!dir.exists("02_output")) {
  dir.create("02_output")
}
```

# Exercises with the synthetic data

## Chapter 9, SQL and R

``` r
# These exercises use SQL to answer simple questions about the OMOP CDM

# 1. How many people are in the database
sql <- "SELECT COUNT(*) AS person_count
FROM @cdm.person;"

renderTranslateQuerySql(connection, sql, cdm = "main") # 2694
```

    ##   person_count
    ## 1         2694

``` r
# 2. How many people have at least one prescription of celecoxib?
sql <- "SELECT COUNT(DISTINCT(person_id)) AS person_count
FROM @cdm.drug_exposure
INNER JOIN @cdm.concept_ancestor
  ON drug_concept_id = descendant_concept_id
INNER JOIN @cdm.concept ingredient
  ON ancestor_concept_id = ingredient.concept_id
WHERE LOWER(ingredient.concept_name) = 'celecoxib'
  AND ingredient.concept_class_id = 'Ingredient'
  AND ingredient.standard_concept = 'S';"

renderTranslateQuerySql(connection, sql, cdm = "main") # 1844
```

    ##   person_count
    ## 1         1844

``` r
# 3. How many diagnoses of gastrointestinal hemorrhage occur during exposure to celecoxib?
sql <- "SELECT COUNT(*) AS diagnose_count
FROM @cdm.drug_era
INNER JOIN @cdm.concept ingredient
  ON drug_concept_id = ingredient.concept_id
INNER JOIN @cdm.condition_occurrence
  ON condition_start_date >= drug_era_start_date
    AND condition_start_date <= drug_era_end_date
INNER JOIN @cdm.concept_ancestor
  ON condition_concept_id =descendant_concept_id
WHERE LOWER(ingredient.concept_name) = 'celecoxib'
  AND ingredient.concept_class_id = 'Ingredient'
  AND ingredient.standard_concept = 'S'
  AND ancestor_concept_id = 192671;"

renderTranslateQuerySql(connection, sql, cdm = "main") # 41
```

    ##   diagnose_count
    ## 1             41

## Chapter 10, Defining a Cohort

``` r
# Create a cohort for acute myocardial infarction (AMI) in the existing COHORT table, following these criteria:

# 1) An occurrence of a myocardial infarction diagnose (concept 4329847 “Myocardial infarction” and all of its descendants, excluding concept 314666 “Old myocardial infarction” and any of its descendants).

# 2) During an inpatient or ER visit (concepts 9201, 9203, and 262 for “Inpatient visit”, “Emergency Room Visit”, and “Emergency Room and Inpatient Visit”, respectively).


# For 1, find all AMI condition occurrences and store these in a temp table called “#diagnoses”
sql <- "SELECT person_id AS subject_id,
  condition_start_date AS cohort_start_date
INTO #diagnoses
FROM @cdm.condition_occurrence
WHERE condition_concept_id IN (
    SELECT descendant_concept_id
    FROM @cdm.concept_ancestor
    WHERE ancestor_concept_id = 4329847 -- Myocardial infarction
)
  AND condition_concept_id NOT IN (
    SELECT descendant_concept_id
    FROM @cdm.concept_ancestor
    WHERE ancestor_concept_id = 314666 -- Old myocardial infarction
);"

renderTranslateExecuteSql(connection, sql, cdm = "main")
```

    ##   |                                                                              |                                                                      |   0%  |                                                                              |===================================                                   |  50%  |                                                                              |======================================================================| 100%

    ## Executing SQL took 0.0232 secs

``` r
# Then for 2, select  those that occur during an inpatient or ER visit, using some unique COHORT_DEFINITION_ID (here selected ‘1’)
sql <- "INSERT INTO @cdm.cohort (
  subject_id,
  cohort_start_date,
  cohort_definition_id
  )
SELECT subject_id,
  cohort_start_date,
  CAST (1 AS INT) AS cohort_definition_id
FROM #diagnoses
INNER JOIN @cdm.visit_occurrence
  ON subject_id = person_id
    AND cohort_start_date >= visit_start_date
    AND cohort_start_date <= visit_end_date
WHERE visit_concept_id IN (9201, 9203, 262); -- Inpatient or ER;"

renderTranslateExecuteSql(connection, sql, cdm = "main")
```

    ##   |                                                                              |                                                                      |   0%  |                                                                              |======================================================================| 100%

    ## Executing SQL took 0.00697 secs

## Chapter 12, Population-Level Estimation

``` r
# Estimate the risk of gastrointestinal (GI) bleeding in new users of
# celecoxib compared with new users of diclofenac.


# celecoxib new-user cohort COHORT_DEFINITION_ID = 1
# diclofenac new-user cohort COHORT_DEFINITION_ID = 2
# GI bleed cohort COHORT_DEFINITION_ID = 3
# concept ID for celecoxib is 1118084
# diclofenac  ID for celecoxib 1124300,

# 1. Define covariates and extract CohortMethodData

nsaids <- c(1118084, 1124300) # celecoxib, diclofenac

covSettings <- createDefaultCovariateSettings(
  excludedCovariateConceptIds = nsaids,
  addDescendantsToExclude = TRUE)

# 2. Extract CohortMethodData from the CDM

# Troubleshooting...
# Different versions of CohortMethod use different function interfaces.
args(getDbCohortMethodData)
```

    ## function (connectionDetails, cdmDatabaseSchema, tempEmulationSchema = getOption("sqlRenderTempEmulationSchema"), 
    ##     targetId, comparatorId, outcomeIds, exposureDatabaseSchema = cdmDatabaseSchema, 
    ##     exposureTable = "drug_era", outcomeDatabaseSchema = cdmDatabaseSchema, 
    ##     outcomeTable = "condition_occurrence", nestingCohortDatabaseSchema = cdmDatabaseSchema, 
    ##     nestingCohortTable = "cohort", getDbCohortMethodDataArgs = createGetDbCohortMethodDataArgs()) 
    ## NULL

``` r
args(createGetDbCohortMethodDataArgs)
```

    ## function (removeDuplicateSubjects = "keep first, truncate to second", 
    ##     firstExposureOnly = TRUE, washoutPeriod = 365, nestingCohortId = NULL, 
    ##     restrictToCommonPeriod = TRUE, minAge = NULL, maxAge = NULL, 
    ##     genderConceptIds = NULL, studyStartDate = "", studyEndDate = "", 
    ##     maxCohortSize = 0, covariateSettings) 
    ## NULL

``` r
# Fix it
# In this version, covariateSettings has to be via createGetDbCohortMethodDataArgs()
getDbCohortMethodDataArgs <- createGetDbCohortMethodDataArgs(
  covariateSettings = covSettings
)

# Back to extracting the data
cmData <- getDbCohortMethodData(
  connectionDetails = connectionDetails,
  cdmDatabaseSchema = "main",
  targetId = 1,
  comparatorId = 2,
  outcomeIds = 3,
  exposureDatabaseSchema = "main",
  exposureTable = "cohort",
  outcomeDatabaseSchema = "main",
  outcomeTable = "cohort",
  getDbCohortMethodDataArgs = getDbCohortMethodDataArgs
)
```

    ## Connecting using SQLite driver
    ## Constructing target and comparator cohorts

    ##   |                                                                              |                                                                      |   0%  |                                                                              |=======================                                               |  33%  |                                                                              |===============================================                       |  67%  |                                                                              |======================================================================| 100%

    ## Executing SQL took 0.0387 secs
    ## Fetching cohorts from server

    ## Fetched cohort total rows in target is 1797, total rows in comparator is 830

    ## Fetching cohorts took 0.0544 secs

    ## Sending temp tables to server

    ## Inserting data took 0.017 secs

    ## Constructing features on server
    ##   |                                                                              |                                                                      |   0%  |                                                                              |=                                                                     |   1%  |                                                                              |=                                                                     |   2%  |                                                                              |==                                                                    |   2%  |                                                                              |==                                                                    |   3%  |                                                                              |===                                                                   |   4%  |                                                                              |===                                                                   |   5%  |                                                                              |====                                                                  |   5%  |                                                                              |====                                                                  |   6%  |                                                                              |=====                                                                 |   7%  |                                                                              |=====                                                                 |   8%  |                                                                              |======                                                                |   8%  |                                                                              |======                                                                |   9%  |                                                                              |=======                                                               |   9%  |                                                                              |=======                                                               |  10%  |                                                                              |========                                                              |  11%  |                                                                              |========                                                              |  12%  |                                                                              |=========                                                             |  12%  |                                                                              |=========                                                             |  13%  |                                                                              |==========                                                            |  14%  |                                                                              |==========                                                            |  15%  |                                                                              |===========                                                           |  15%  |                                                                              |===========                                                           |  16%  |                                                                              |============                                                          |  17%  |                                                                              |============                                                          |  18%  |                                                                              |=============                                                         |  18%  |                                                                              |=============                                                         |  19%  |                                                                              |==============                                                        |  19%  |                                                                              |==============                                                        |  20%  |                                                                              |==============                                                        |  21%  |                                                                              |===============                                                       |  21%  |                                                                              |===============                                                       |  22%  |                                                                              |================                                                      |  22%  |                                                                              |================                                                      |  23%  |                                                                              |================                                                      |  24%  |                                                                              |=================                                                     |  24%  |                                                                              |=================                                                     |  25%  |                                                                              |==================                                                    |  25%  |                                                                              |==================                                                    |  26%  |                                                                              |===================                                                   |  26%  |                                                                              |===================                                                   |  27%  |                                                                              |===================                                                   |  28%  |                                                                              |====================                                                  |  28%  |                                                                              |====================                                                  |  29%  |                                                                              |=====================                                                 |  29%  |                                                                              |=====================                                                 |  30%  |                                                                              |=====================                                                 |  31%  |                                                                              |======================                                                |  31%  |                                                                              |======================                                                |  32%  |                                                                              |=======================                                               |  32%  |                                                                              |=======================                                               |  33%  |                                                                              |========================                                              |  34%  |                                                                              |========================                                              |  35%  |                                                                              |=========================                                             |  35%  |                                                                              |=========================                                             |  36%  |                                                                              |==========================                                            |  37%  |                                                                              |==========================                                            |  38%  |                                                                              |===========================                                           |  38%  |                                                                              |===========================                                           |  39%  |                                                                              |============================                                          |  40%  |                                                                              |============================                                          |  41%  |                                                                              |=============================                                         |  41%  |                                                                              |=============================                                         |  42%  |                                                                              |==============================                                        |  42%  |                                                                              |==============================                                        |  43%  |                                                                              |===============================                                       |  44%  |                                                                              |===============================                                       |  45%  |                                                                              |================================                                      |  45%  |                                                                              |================================                                      |  46%  |                                                                              |=================================                                     |  47%  |                                                                              |=================================                                     |  48%  |                                                                              |==================================                                    |  48%  |                                                                              |==================================                                    |  49%  |                                                                              |===================================                                   |  50%  |                                                                              |====================================                                  |  51%  |                                                                              |====================================                                  |  52%  |                                                                              |=====================================                                 |  52%  |                                                                              |=====================================                                 |  53%  |                                                                              |======================================                                |  54%  |                                                                              |======================================                                |  55%  |                                                                              |=======================================                               |  55%  |                                                                              |=======================================                               |  56%  |                                                                              |========================================                              |  57%  |                                                                              |========================================                              |  58%  |                                                                              |=========================================                             |  58%  |                                                                              |=========================================                             |  59%  |                                                                              |==========================================                            |  59%  |                                                                              |==========================================                            |  60%  |                                                                              |===========================================                           |  61%  |                                                                              |===========================================                           |  62%  |                                                                              |============================================                          |  62%  |                                                                              |============================================                          |  63%  |                                                                              |=============================================                         |  64%  |                                                                              |=============================================                         |  65%  |                                                                              |==============================================                        |  65%  |                                                                              |==============================================                        |  66%  |                                                                              |===============================================                       |  67%  |                                                                              |===============================================                       |  68%  |                                                                              |================================================                      |  68%  |                                                                              |================================================                      |  69%  |                                                                              |=================================================                     |  69%  |                                                                              |=================================================                     |  70%  |                                                                              |=================================================                     |  71%  |                                                                              |==================================================                    |  71%  |                                                                              |==================================================                    |  72%  |                                                                              |===================================================                   |  72%  |                                                                              |===================================================                   |  73%  |                                                                              |===================================================                   |  74%  |                                                                              |====================================================                  |  74%  |                                                                              |====================================================                  |  75%  |                                                                              |=====================================================                 |  75%  |                                                                              |=====================================================                 |  76%  |                                                                              |======================================================                |  76%  |                                                                              |======================================================                |  77%  |                                                                              |======================================================                |  78%  |                                                                              |=======================================================               |  78%  |                                                                              |=======================================================               |  79%  |                                                                              |========================================================              |  79%  |                                                                              |========================================================              |  80%  |                                                                              |========================================================              |  81%  |                                                                              |=========================================================             |  81%  |                                                                              |=========================================================             |  82%  |                                                                              |==========================================================            |  82%  |                                                                              |==========================================================            |  83%  |                                                                              |===========================================================           |  84%  |                                                                              |===========================================================           |  85%  |                                                                              |============================================================          |  85%  |                                                                              |============================================================          |  86%  |                                                                              |=============================================================         |  87%  |                                                                              |=============================================================         |  88%  |                                                                              |==============================================================        |  88%  |                                                                              |==============================================================        |  89%  |                                                                              |===============================================================       |  90%  |                                                                              |===============================================================       |  91%  |                                                                              |================================================================      |  91%  |                                                                              |================================================================      |  92%  |                                                                              |=================================================================     |  92%  |                                                                              |=================================================================     |  93%  |                                                                              |==================================================================    |  94%  |                                                                              |==================================================================    |  95%  |                                                                              |===================================================================   |  95%  |                                                                              |===================================================================   |  96%  |                                                                              |====================================================================  |  97%  |                                                                              |====================================================================  |  98%  |                                                                              |===================================================================== |  98%  |                                                                              |===================================================================== |  99%  |                                                                              |======================================================================| 100%

    ## Executing SQL took 1.73 secs

    ## Fetching data from server
    ## Fetching data took 0.681 secs
    ## Fetched covariates total count is 26893

    ## Fetching outcomes from server
    ## Fetching outcomes took 0.0254 secs

    ## Fetched outcomes total count is 479

``` r
# Look at the data
summary(cmData)
```

    ## CohortMethodData object summary
    ## 
    ## Target cohort ID: 1
    ## Comparator cohort ID: 2
    ## Outcome cohort ID(s): 3
    ## 
    ## Target persons: 1797
    ## Comparator persons: 830
    ## 
    ## Outcome counts:

    ##   Event count Person count
    ## 3         479          479

    ## 
    ## Covariates:
    ## Number of covariates: 388
    ## Number of non-zero covariate values: 26893

``` r
# 3. Define the study population
# exclude subjects with a prior GI bleed
# start the risk window at cohort start
# follow subjects through the end of their cohort observation period

# Troubleshooting
args(createStudyPopulation)
```

    ## function (plpData, outcomeId = plpData$metaData$databaseDetails$outcomeIds[1], 
    ##     populationSettings = createStudyPopulationSettings(), population = NULL) 
    ## NULL

``` r
args(createCreateStudyPopulationArgs)
```

    ## function (removeSubjectsWithPriorOutcome = TRUE, priorOutcomeLookback = 99999, 
    ##     minDaysAtRisk = 1, maxDaysAtRisk = 99999, riskWindowStart = 0, 
    ##     startAnchor = "cohort start", riskWindowEnd = 0, endAnchor = "cohort end", 
    ##     censorAtNewRiskWindow = FALSE) 
    ## NULL

``` r
# Study population
studyPop <- CohortMethod::createStudyPopulation(
  cohortMethodData = cmData,
  outcomeId = 3,
  createStudyPopulationArgs = CohortMethod::createCreateStudyPopulationArgs(
    removeSubjectsWithPriorOutcome = TRUE,
    riskWindowStart = 0,
    startAnchor = "cohort start",
    riskWindowEnd = 99999,
    endAnchor = "cohort end"
  )
)
```

    ## Removing subjects with prior outcomes (if any)
    ## Removing subjects with less than 1 day(s) at risk (if any)

    ## Study population has 2627 rows

``` r
# 4. Examine study-population attrition

# Draw the attrition/flowchart
drawAttritionDiagram(studyPop)
```

![](02_exercises_files/figure-gfm/ch.%2012%20exercises-1.png)<!-- -->

``` r
# 5. Fit an unadjusted Cox proportional hazards model

# Troubleshoot
args(fitOutcomeModel)
```

    ## function (population, cohortMethodData = NULL, fitOutcomeModelArgs = createFitOutcomeModelArgs()) 
    ## NULL

``` r
args(createFitOutcomeModelArgs)
```

    ## function (modelType = "cox", stratified = FALSE, useCovariates = FALSE, 
    ##     inversePtWeighting = FALSE, bootstrapCi = FALSE, bootstrapReplicates = 200, 
    ##     interactionCovariateIds = c(), excludeCovariateIds = c(), 
    ##     includeCovariateIds = c(), profileGrid = NULL, profileBounds = c(log(0.1), 
    ##         log(10)), prior = createPrior(priorType = "laplace", 
    ##         useCrossValidation = TRUE), control = createControl(cvType = "auto", 
    ##         seed = 1, resetCoefficients = TRUE, startingVariance = 0.01, 
    ##         tolerance = 2e-07, cvRepetitions = 10, noiseLevel = "quiet")) 
    ## NULL

``` r
cox_model <- fitOutcomeModel(
  population = studyPop,
  fitOutcomeModelArgs = createFitOutcomeModelArgs(
    modelType = "cox"
  )
)
```

    ## Using prior: None 
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)

    ## Fitting outcome model took 0.699 secs

    ## Outcome model fitting status is: OK

``` r
# Look at the results
cox_model
```

    ## Model type: cox
    ## Stratified: FALSE
    ## Use covariates: FALSE
    ## Use inverse probability of treatment weighting: FALSE
    ## Target estimand: ate
    ## Status: OK

    ##           Estimate lower .95 upper .95   logRr seLogRr
    ## treatment  1.34862   1.10268   1.66049 0.29909  0.1044

``` r
# 6. Estimate the propensity score with covariates from above

ps <- createPs(cohortMethodData = cmData,
               population = studyPop)
```

    ## Removing 6 redundant covariates
    ## Removing 112 infrequent covariates
    ## Normalizing covariates
    ## Tidying covariates took 3.49 secs
    ## Propensity model fitting finished with status OK

    ## Creating propensity scores took 10.1 secs

``` r
# Plot the preference score distribution
plotPs(ps, showCountsLabel = TRUE, showAucLabel = TRUE)
```

![](02_exercises_files/figure-gfm/ch.%2012%20exercises-2.png)<!-- -->

``` r
# 7. Stratify on the propensity score and assess covariate balance

strataPop <- stratifyByPs(ps) # Divide subjects into propensity-score strata

# Now look at covariate balance before and after propensity-score stratification

bal <- computeCovariateBalance(strataPop, cmData)
```

    ## Computing covariate balance took 3.76 secs

``` r
# Plot the standardized covariate balance
plotCovariateBalanceScatterPlot(bal,
                                showCovariateCountLabel = TRUE,
                                showMaxLabel = TRUE,
                                beforeLabel = "Before stratification",
                                afterLabel = "After stratification")
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_hline()`).

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_vline()`).

![](02_exercises_files/figure-gfm/ch.%2012%20exercises-3.png)<!-- -->

``` r
# 8. Fit a Cox proportional hazards model using the propensity score strata
adjModel <- fitOutcomeModel(
  population = strataPop,
  fitOutcomeModelArgs = createFitOutcomeModelArgs(
    modelType = "cox",
    stratified = TRUE
  )
)
```

    ## Using prior: None 
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)
    ## Using 1 thread(s)

    ## Fitting outcome model took 0.662 secs

    ## Outcome model fitting status is: OK

``` r
# Look at the results
adjModel
```

    ## Model type: cox
    ## Stratified: TRUE
    ## Use covariates: FALSE
    ## Use inverse probability of treatment weighting: FALSE
    ## Target estimand: ate
    ## Status: OK

    ##           Estimate lower .95 upper .95    logRr seLogRr
    ## treatment 1.083058  0.880878  1.340097 0.079788   0.107

## Chapter 13, Patient-Level Prediction

``` r
# Predict which patients who are new users of NSAIDs will develop a
# gastrointestinal (GI) bleed during the following year.

# Study cohorts:
#   Target/exposure cohort: new NSAID users (COHORT_DEFINITION_ID = 4)
#   Outcome cohort: GI bleed (COHORT_DEFINITION_ID = 3)

# 1. Define the covariates and extract the PLP data

# Define the baseline characteristics that will be used as predictors

covSettings <- createCovariateSettings(
  useDemographicsGender = TRUE,
  useDemographicsAge = TRUE,
  useConditionGroupEraLongTerm = TRUE,
  useConditionGroupEraAnyTimePrior = TRUE,
  useDrugGroupEraLongTerm = TRUE,
  useDrugGroupEraAnyTimePrior = TRUE,
  useVisitConceptCountLongTerm = TRUE,
  longTermStartDays = -365,
  endDays = -1
)

# Troubleshooting; inspect the API for the installed PLP version

args(getPlpData)
```

    ## function (databaseDetails, covariateSettings, restrictPlpDataSettings = NULL) 
    ## NULL

``` r
args(createRestrictPlpDataSettings)
```

    ## function (studyStartDate = "", studyEndDate = "", firstExposureOnly = FALSE, 
    ##     washoutPeriod = 0, sampleSize = NULL) 
    ## NULL

``` r
args(createDatabaseDetails)
```

    ## function (connectionDetails, cdmDatabaseSchema, cdmDatabaseName, 
    ##     cdmDatabaseId, tempEmulationSchema = cdmDatabaseSchema, cohortDatabaseSchema = cdmDatabaseSchema, 
    ##     cohortTable = "cohort", outcomeDatabaseSchema = cohortDatabaseSchema, 
    ##     outcomeTable = cohortTable, targetId = NULL, outcomeIds = NULL, 
    ##     cdmVersion = 5, cohortId = NULL) 
    ## NULL

``` r
# Create the database configuration used by getPlpData().

# cohortId = 4 identifies the NSAID new-user target cohort.
# outcomeIds = 3 identifies the GI bleed outcome cohort.

# The CDM, target cohort, and outcome cohort are all stored in the "main.cohort" table in this study

databaseDetails <- createDatabaseDetails(
  connectionDetails = connectionDetails,
  cdmDatabaseSchema = "main",
  cdmDatabaseName = "main",
  cdmDatabaseId = 1,       # Replace with the actual CDM database ID if needed
  cohortDatabaseSchema = "main",
  cohortTable = "cohort",
  outcomeDatabaseSchema = "main",
  outcomeTable = "cohort",
  cohortId = 4,
  outcomeIds = 3
)
```

    ## cdmDatabaseId is not a string - this will cause issues when inserting into a result database so casting it

``` r
# Extract the PLP data from the CDM using the database configuration
# and the covariate definitions above.

plpData <- getPlpData(
  databaseDetails = databaseDetails,
  covariateSettings = covSettings
)
```

    ## Connecting using SQLite driver

    ## 
    ## Constructing the at risk cohort
    ##   |                                                                              |                                                                      |   0%  |                                                                              |=======================                                               |  33%  |                                                                              |===============================================                       |  67%  |                                                                              |======================================================================| 100%

    ## Executing SQL took 0.026 secs

    ## Fetching cohorts from server
    ## Loading cohorts took 0.045 secs
    ## Constructing features on server
    ##   |                                                                              |                                                                      |   0%  |                                                                              |=                                                                     |   2%  |                                                                              |===                                                                   |   4%  |                                                                              |====                                                                  |   6%  |                                                                              |=====                                                                 |   8%  |                                                                              |=======                                                               |  10%  |                                                                              |========                                                              |  12%  |                                                                              |=========                                                             |  13%  |                                                                              |===========                                                           |  15%  |                                                                              |============                                                          |  17%  |                                                                              |=============                                                         |  19%  |                                                                              |===============                                                       |  21%  |                                                                              |================                                                      |  23%  |                                                                              |==================                                                    |  25%  |                                                                              |===================                                                   |  27%  |                                                                              |====================                                                  |  29%  |                                                                              |======================                                                |  31%  |                                                                              |=======================                                               |  33%  |                                                                              |========================                                              |  35%  |                                                                              |==========================                                            |  37%  |                                                                              |===========================                                           |  38%  |                                                                              |============================                                          |  40%  |                                                                              |==============================                                        |  42%  |                                                                              |===============================                                       |  44%  |                                                                              |================================                                      |  46%  |                                                                              |==================================                                    |  48%  |                                                                              |===================================                                   |  50%  |                                                                              |====================================                                  |  52%  |                                                                              |======================================                                |  54%  |                                                                              |=======================================                               |  56%  |                                                                              |========================================                              |  58%  |                                                                              |==========================================                            |  60%  |                                                                              |===========================================                           |  62%  |                                                                              |============================================                          |  63%  |                                                                              |==============================================                        |  65%  |                                                                              |===============================================                       |  67%  |                                                                              |================================================                      |  69%  |                                                                              |==================================================                    |  71%  |                                                                              |===================================================                   |  73%  |                                                                              |====================================================                  |  75%  |                                                                              |======================================================                |  77%  |                                                                              |=======================================================               |  79%  |                                                                              |=========================================================             |  81%  |                                                                              |==========================================================            |  83%  |                                                                              |===========================================================           |  85%  |                                                                              |=============================================================         |  87%  |                                                                              |==============================================================        |  88%  |                                                                              |===============================================================       |  90%  |                                                                              |=================================================================     |  92%  |                                                                              |==================================================================    |  94%  |                                                                              |===================================================================   |  96%  |                                                                              |===================================================================== |  98%  |                                                                              |======================================================================| 100%

    ## Executing SQL took 1.52 secs

    ## Fetching data from server
    ## Fetching data took 0.951 secs
    ## Fetching outcomes from server
    ## Loading outcomes took 0.0505 secs

``` r
# Look at summary of the data

summary(plpData)
```

    ## plpData object summary
    ## 
    ## At risk cohort concept ID: 4
    ## Outcome concept ID(s): 3
    ## 
    ## People: 2630
    ## 
    ## Outcome counts:
    ##   Event count Person count
    ## 3         479          479
    ## 
    ## Covariates:
    ## Number of covariates: 245
    ## Number of non-zero covariate values: 54079

``` r
# 2. Define the prediction-study population
# The study population is defined around the NSAID cohort entry.

# Troubleshooting; look at the API used by the installed PLP version.

args(createStudyPopulation)
```

    ## function (plpData, outcomeId = plpData$metaData$databaseDetails$outcomeIds[1], 
    ##     populationSettings = createStudyPopulationSettings(), population = NULL) 
    ## NULL

``` r
args(createStudyPopulationSettings)
```

    ## function (binary = TRUE, includeAllOutcomes = TRUE, firstExposureOnly = FALSE, 
    ##     washoutPeriod = 0, removeSubjectsWithPriorOutcome = TRUE, 
    ##     priorOutcomeLookback = 99999, requireTimeAtRisk = TRUE, minTimeAtRisk = 364, 
    ##     riskWindowStart = 1, startAnchor = "cohort start", riskWindowEnd = 365, 
    ##     endAnchor = "cohort start", restrictTarToCohortEnd = FALSE) 
    ## NULL

``` r
formals(createStudyPopulation)
```

    ## $plpData
    ## 
    ## 
    ## $outcomeId
    ## plpData$metaData$databaseDetails$outcomeIds[1]
    ## 
    ## $populationSettings
    ## createStudyPopulationSettings()
    ## 
    ## $population
    ## NULL

``` r
# createStudyPopulation() returns the actual study population
# createStudyPopulationSettings() defines the rules used to construct it.

plpStudyPop <- createStudyPopulation(
  plpData,
  3,
  createStudyPopulationSettings(
    washoutPeriod = 364,
    firstExposureOnly = FALSE,
    removeSubjectsWithPriorOutcome = TRUE,
    priorOutcomeLookback = 9999,
    riskWindowStart = 1,
    startAnchor = "cohort start",
    riskWindowEnd = 365,
    endAnchor = "cohort start",
    minTimeAtRisk = 364,
    requireTimeAtRisk = TRUE,
    includeAllOutcomes = TRUE
  )
)
```

    ## outcomeId: 3
    ## binary: TRUE
    ## includeAllOutcomes: TRUE
    ## firstExposureOnly: FALSE
    ## washoutPeriod: 364
    ## removeSubjectsWithPriorOutcome: TRUE
    ## priorOutcomeLookback: 9999
    ## requireTimeAtRisk: TRUE
    ## minTimeAtRisk: 364
    ## restrictTarToCohortEnd: FALSE
    ## riskWindowStart: 1
    ## startAnchor: cohort start
    ## riskWindowEnd: 365
    ## endAnchor: cohort start
    ## restrictTarToCohortEnd: FALSE
    ## Requiring 364 days of observation prior index date
    ## Removing subjects with prior outcomes (if any)
    ## Removing non outcome subjects with insufficient time at risk (if any)
    ## Outcome is 0 or 1
    ## Population created with: 2578 observations, 2578 unique subjects and 479 outcomes
    ## Population created in 0.0778 secs

``` r
# Check how many patients remain in the final prediction population.

nrow(plpStudyPop)
```

    ## [1] 2578

``` r
# 3. Specify the prediction model
# Use LASSO logistic regression.

lassoModel <- setLassoLogisticRegression(seed = 0)

# 4. Define the train/test split

# Troubleshooting; inspect the arguments accepted by runPlp() in the
# installed version of PatientLevelPrediction.

args(runPlp)
```

    ## function (plpData, outcomeId = plpData$metaData$databaseDetails$outcomeIds[1], 
    ##     analysisId = paste(Sys.Date(), outcomeId, sep = "-"), analysisName = "Study details", 
    ##     populationSettings = createStudyPopulationSettings(), splitSettings = createDefaultSplitSetting(type = "stratified", 
    ##         testFraction = 0.25, trainFraction = 0.75, splitSeed = 123, 
    ##         nfold = 3), sampleSettings = createSampleSettings(type = "none"), 
    ##     featureEngineeringSettings = createFeatureEngineeringSettings(type = "none"), 
    ##     preprocessSettings = createPreprocessSettings(minFraction = 0.001, 
    ##         normalize = TRUE), modelSettings = setLassoLogisticRegression(), 
    ##     hyperparameterSettings = createHyperparameterSettings(), 
    ##     logSettings = createLogSettings(verbosity = "DEBUG", timeStamp = TRUE, 
    ##         logName = "runPlp Log"), executeSettings = createDefaultExecuteSettings(), 
    ##     saveDirectory = NULL) 
    ## NULL

``` r
# Use a subject-level split 75/25 split
splitSettings <- createDefaultSplitSetting(
  type = "subject",
  testFraction = 0.25,
  trainFraction = 0.75,
  splitSeed = 123456,
  nfold = 2
)


# 5. Specify the population settings for runPlp()

# runPlp() expects the population settings
# So use the same study-population definition used above 

populationSettings <- createStudyPopulationSettings(
  washoutPeriod = 364,
  firstExposureOnly = FALSE,
  removeSubjectsWithPriorOutcome = TRUE,
  priorOutcomeLookback = 9999,
  riskWindowStart = 1,
  startAnchor = "cohort start",
  riskWindowEnd = 365,
  endAnchor = "cohort start",
  minTimeAtRisk = 364,
  requireTimeAtRisk = TRUE,
  includeAllOutcomes = TRUE
)


# 6. Run the LASSO prediction model

lassoResults <- runPlp(
  plpData = plpData,
  populationSettings = populationSettings,
  modelSettings = lassoModel,
  splitSettings = splitSettings,
  saveDirectory = "02_output"
)
```

    ## Use timeStamp: TRUE

    ## Currently in a tryCatch or withCallingHandlers block, so unable to add global calling handlers. ParallelLogger will not capture R messages, errors, and warnings, only explicit calls to ParallelLogger. (This message will not be shown again this R session)

    ## Patient-Level Prediction Package version 6.7.0
    ## Study started at: 2026-09-26 21:59:02.840079
    ## AnalysisID:         2026-09-26-3
    ## AnalysisName:       Study details
    ## TargetID:           4
    ## OutcomeID:          3
    ## Cohort size:        2630
    ## Covariates:         245
    ## Creating population
    ## Outcome is 0 or 1
    ## Population created with: 2578 observations, 2578 unique subjects and 479 outcomes
    ## Population created in 0.122 secs
    ## seed: 123456
    ## Creating a 25% test and 75% train (into 2 folds) stratified split by subject
    ## Data split into 645 test cases and 1933 train cases (967, 966)
    ## Data split in 3.67 secs
    ## Train Set:
    ## Fold 1 967 patients with 180 outcomes - Fold 2 966 patients with 179 outcomes
    ## 240 covariates in train data
    ## Test Set:
    ## 645 patients with 120 outcomes
    ## Removing 1 redundant covariates
    ## Removing 0 infrequent covariates
    ## Normalizing covariates
    ## Tidying covariates took 4.21 secs
    ## Train Set:
    ## Fold 1 967 patients with 180 outcomes - Fold 2 966 patients with 179 outcomes
    ## 239 covariates in train data
    ## Test Set:
    ## 645 patients with 120 outcomes
    ## 
    ## Running Cyclops
    ## Done.
    ## GLM fit status:  OK
    ## Creating variable importance data frame
    ## Prediction took 0.511 secs
    ## Time to fit model: 2.26 secs
    ## Removing infrequent and redundant covariates and normalizing
    ## Removing infrequent and redundant covariates covariates and normalizing took 1.37 secs
    ## Prediction took 0.376 secs
    ## Prediction done in: 3.2 secs
    ## Calculating Performance for Test
    ## =============
    ## AUC                 67.06
    ## 95% lower AUC:      61.50
    ## 95% upper AUC:      72.61
    ## AUPRC:              30.84
    ## Brier:              0.14
    ## Eavg:               0.01
    ## Calibration in large- Mean predicted risk 0.1875 : observed risk 0.186
    ## Calibration in large- Intercept 0.0317
    ## Weak calibration intercept: 0.0317 - gradient:1.0307
    ## Hosmer-Lemeshow calibration gradient: 1.02 intercept:         -0.00
    ## Average Precision:  0.31
    ## Calculating Performance for Train
    ## =============
    ## AUC                 70.73
    ## 95% lower AUC:      67.75
    ## 95% upper AUC:      73.70
    ## AUPRC:              35.26
    ## Brier:              0.14
    ## Eavg:               0.02
    ## Calibration in large- Mean predicted risk 0.1857 : observed risk 0.1857
    ## Calibration in large- Intercept 0.221
    ## Weak calibration intercept: 0.221 - gradient:1.1633
    ## Hosmer-Lemeshow calibration gradient: 1.16 intercept:         -0.03
    ## Average Precision:  0.35
    ## Calculating Performance for CV
    ## =============
    ## AUC                 66.14
    ## 95% lower AUC:      62.95
    ## 95% upper AUC:      69.32
    ## AUPRC:              28.84
    ## Brier:              0.14
    ## Eavg:               0.02
    ## Calibration in large- Mean predicted risk 0.1861 : observed risk 0.1857
    ## Calibration in large- Intercept 0.1938
    ## Weak calibration intercept: 0.1938 - gradient:1.1429
    ## Hosmer-Lemeshow calibration gradient: 1.05 intercept:         -0.02
    ## Average Precision:  0.29
    ## Time to calculate evaluation metrics: 0.738 secs
    ## Calculating covariate summary @ 2026-09-26 21:59:18.016792
    ## This can take a while...
    ## Creating binary labels
    ## Joining with strata
    ## calculating subset of strata 1
    ## calculating subset of strata 2
    ## calculating subset of strata 3
    ## calculating subset of strata 4
    ## Restricting to subgroup
    ## Calculating summary for subgroup TrainWithNoOutcome
    ## Restricting to subgroup
    ## Calculating summary for subgroup TrainWithOutcome
    ## Restricting to subgroup
    ## Calculating summary for subgroup TestWithNoOutcome
    ## Restricting to subgroup
    ## Calculating summary for subgroup TestWithOutcome
    ## Aggregating with labels and strata
    ## Finished covariate summary @ 2026-09-26 21:59:24.81975
    ## Time to calculate covariate summary: 6.8 secs
    ## Run finished successfully.
    ## Saving PlpResult
    ## plpResult saved to ..\02_output/2026-09-26-3\plpResult
    ## runPlp time taken: 22.1 secs
