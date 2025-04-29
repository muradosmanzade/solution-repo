# Problem 1

Exploring the Central Limit Theorem Through Simulation Techniques

## 1. Theoretical Foundation

The Central Limit Theorem (CLT) is a fundamental concept in statistics, which asserts that: for a set of independent and identically distributed (i.i.d.) random variables, the distribution of sample means approaches a normal distribution as the sample size increases, irrespective of the original distribution's shape.


## 2. Implementation and Analysis

We will investigate this behavior through Python-based simulations, utilizing various probability distributions:


$$ \bar{X}_n \sim N\left(\mu, \frac{\sigma^2}{n}\right) $$


The standardized version of the sampling distribution is:

$$ \frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} \sim N(0,1) $$


## 3. Implementation and Analysis

We will examine this behavior through Python-based simulations using various probability distributions:

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats## 2. Implementation and Analysis

We will investigate this behavior through Python-based simulations, utilizing various probability distributions:

# Set random seed for reproducibility
np.random.seed(42)

# For Colab, we need to use this to display plots
%matplotlib inline

# Plotting style configuration
sns.set_style("whitegrid")  # This is more reliable than plt.style.use('seaborn')
plt.rcParams['figure.figsize'] = (12, 8)

def plot_sampling_distribution(population, sample_sizes, n_samples=1000, title=""):
    fig, axes = plt.subplots(2, 2, figsize=(15, 12))
    fig.suptitle(f'Sampling Distributions: {title}', fontsize=16)
    
    for i, n in enumerate(sample_sizes):
        row = i // 2
        col = i % 2
        
        # Generate sample means
        sample_means = [np.mean(np.random.choice(population, size=n)) 
                       for _ in range(n_samples)]
        
        # Plot histogram with KDE
        sns.histplot(sample_means, kde=True, ax=axes[row, col])
        
        # Add normal distribution fit
        mu = np.mean(sample_means)
        sigma = np.std(sample_means)
        x = np.linspace(min(sample_means), max(sample_means), 100)
        normal_dist = stats.norm.pdf(x, mu, sigma)
        axes[row, col].plot(x, normal_dist * len(sample_means) * 
                          (max(sample_means) - min(sample_means)) / 30,
                          'r--', lw=2, label='Normal Fit')
        
        # Add theoretical values
        axes[row, col].axvline(np.mean(population), color='g', 
                              linestyle='--', label='Population Mean')
        
        axes[row, col].set_title(f'Sample Size = {n}\nμ={mu:.2f}, σ={sigma:.2f}')
        axes[row, col].set_xlabel('Sample Mean')
        axes[row, col].set_ylabel('Frequency')
        axes[row, col].legend()
    
    plt.tight_layout()
    return fig

# 1. Uniform Distribution
uniform_pop = np.random.uniform(0, 10, 10000)
fig1 = plot_sampling_distribution(uniform_pop, [5, 10, 30, 50], 
                                title="Uniform Distribution (0, 10)")
plt.show()
```

![Uniform Distribution](images/problem%201.2.0.PNG  )


## 4. Standard Error Analysis

The CLT predicts that the standard error (SE) of the sampling distribution decreases with the square root of the sample size:

$$ SE = \frac{\sigma}{\sqrt{n}} $$

```python
def plot_standard_error(population, max_sample_size=100):
    sample_sizes = np.arange(5, max_sample_size + 1, 5)
    theoretical_se = np.std(population) / np.sqrt(sample_sizes)
    empirical_se = []
    
    for n in sample_sizes:
        sample_means = [np.mean(np.random.choice(population, size=n)) 
                       for _ in range(1000)]
        empirical_se.append(np.std(sample_means))
    
    plt.figure(figsize=(10, 6))
    plt.plot(sample_sizes, theoretical_se, 'r-', label='Theoretical SE')
    plt.plot(sample_sizes, empirical_se, 'b--', label='Empirical SE')
    plt.xlabel('Sample Size')
    plt.ylabel('Standard Error')
    plt.title('Standard Error vs Sample Size')
    plt.legend()
    plt.grid(True)
    return plt.gcf()

fig5 = plot_standard_error(exponential_pop)
plt.show()
```

![Standard Error vs Sample Size](images/problem%201.4.PNG   )
## 5. Practical Applications

The Central Limit Theorem (CLT) has a variety of real-world uses:

### Quality Control
- Overseeing manufacturing processes
- Creating confidence intervals for measurements

### Financial Analysis  
- Evaluating portfolio risk
- Forecasting market returns

### Scientific Research
- Planning experiments
- Drawing statistical conclusions


## 6. Conclusions

Our simulations highlight several key insights about the CLT:

1. The sampling distribution approaches normality as the sample size increases.
2. The rate of convergence is influenced by the shape of the original distribution.
3. The standard error decreases in a predictable manner with increasing sample size.
4. The theorem holds true regardless of the underlying population distribution.

These results have significant implications for statistical inference and experimental design in practical applications.

## References

1. Rice, J. A. (2006). *Mathematical Statistics and Data Analysis*
2. Wasserman, L. (2004). *All of Statistics*
3. Montgomery, D. C. (2009). *Statistical Quality Control*



