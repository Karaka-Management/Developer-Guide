# Statistics

## Definitions

### Range

$$
range = max(values) - min(values)
$$

```php
ArrayUtils::range($values);
```

### Mean (arithmetic)

$$
\mu = mean = \bar{X} = \frac{1}{n}\sum_{i=1}^{n}a_i
$$

```php
Average::arithmeticMean($values);
```

### Variance

$$
\sigma^{2} = Var(X) = \frac{1}{N} \sum_{i = 1}^{N}\left(x_{i} - \bar{X}\right)^{2}
$$

```php
MeasureOfDispersion::empiricalVariance($x, $y);
MeasureOfDispersion::sampleVariance($x, $y);
```

> The sample variance calculates with N - 1

### Covariance

$$
cov(X,Y) = \frac{1}{N} \sum_{i = 1}^{N}\left(x_{i} - \bar{X}\right)\left(y_{i} - \bar{Y}\right)
$$

```php
MeasureOfDispersion::empiricalCovariance($x, $y);
MeasureOfDispersion::sampleCovariance($x, $y);
```

> The sample covariance calculates with N - 1

## Variable types

Variables are characteristics (i.e. height, weight)

### Quantitive

Quantitative variables represent a measer, they are numeric.

#### Discrete

Countable and finite possibilities/results/options.

#### Continuous

Not countable and infinite number of possibilities/results/options.

### Qualitative

Qualitative variables represent categories/levels, they are not numeric.

#### Nominal

Cannot be ordered, they have the same importance/level/value (i.e. hair color). A category can also by yes/no (= two levels).

#### Ordinal

Can be ordered, they have different importance/level/value (i.e. risk severity)

### Transformation

It is possible to transfrom variables:

* Continuous to discrete: by removing steps in between (i.e. age only in years)
* Quantitive to qualitive: by asigning numeric ranges a quality (i.e. school grades in pass and fail)

## Tests

### Chi-square

#### Goodness of fit test

This tests if observed values follows expected/known proportions (e.g. distributions.)

* H0: The observation follows the expected frequency/distribution (i.e. normal distribution)
* H1: The observation doesn't follow the expected frequency/distribution (i.e. normal distribution)

> If H0 can be discarded or not depends on the significance (p-value).

```php
ChiSquaredDistribution::testHypothesis($observed, $expected, $significance = 0.05, $degreesOfFreedom = 0);
```

#### Test of independence

This tests if there is a relationship between two categorical variables.

* H0: The variables are independent (there is no relationship between them)
* H1: The variables are dependent (there is a relationship between them)

### t-test

#### One sample

This tests if the observed mean is different from a expected mean.

#### Two samples

This tests if the observed mean or median is different from two samples.

### Correlation

#### Pearson correlation

Measures how two variables are related to each other (do they perform similarly, opposite or not related at all).

$$
\rho_{XY} = \frac{cov(X, Y)}{\sigma_X \sigma_Y}
$$

```php
Correlation::bravaisPersonCorrelationCoefficientPopulation($x, $y);
Correlation::bravaisPersonCorrelationCoefficientSample($x, $y);
```

> The sample correlation calculates with N - 1
