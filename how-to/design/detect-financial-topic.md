# Detect Financial Topic

The Financial Topic Detection feature is designed to identify whether a user's input mentions any financial topic, such as investment, inflation, or saving money. This functionality enriches applications by allowing for dynamic, content-aware responses based on the financial subjects discussed by the user.

### Implementation

Financial topics detected in user input are stored in the `Input` class within the `financialTopic` variable. This variable is of type `StructuredClass<FinancialTopic>?`, which may be `null` if no financial topic is detected.

#### Accessing Detected Financial Topic

To access the detected financial topic within your application, you can use the following Kotlin code snippet:

```kotlin
val detectedFinancialTopic = input.financialTopic
```

#### Branching Dialogue Based on Financial Topic

To branch dialogues or logic based on the detected financial topic, you can use a conditional check to determine if `detectedFinancialTopic` is not `null`, followed by a `when` statement to handle each specific topic. Below is an example of how to implement such branching:

```kotlin
kotlinCopy codeif (detectedFinancialTopic != null) {
    when (detectedFinancialTopic.value) {
        FinancialTopic.BUDGET_PLANNING -> {
            // Handle BUDGET_PLANNING
        }
        FinancialTopic.HIGH_PRICES -> {
            // Handle HIGH_PRICES
        }
        // Add additional cases for other financial topics
    }
}
```

### Financial Topics

The `FinancialTopic` enum includes a list of predefined topics that can be detected. The following topics are supported:

* `BUDGET_PLANNING`: Discussions related to planning and managing a budget.
* `HIGH_PRICES`: Conversations about high prices or inflation.
* `PAYMENT_CATEGORIZATION`: Identifying different categories of payments or expenses.
* `SAVE_MONEY`: Topics related to saving money or financial advice on savings.
* `SUSTAINABILITY`: Financial discussions related to sustainability and eco-friendly practices.
* `SCAMS`: Identifying mentions of scams, fraud, or deceptive practices.
* `TECHNOLOGY`: Conversations about financial technology, apps, or platforms.
* `ENERGY`: Topics related to energy costs, savings, or investments.
* `HOUSING`: Discussions about housing, mortgages, rent, or real estate investments.
* `RESOLUTION`: Financial resolutions, goals, or planning for the future.
* `SELF_IMPROVEMENT`: Investing in self-improvement, education, or personal growth with financial implications.
* `VACATION`: Planning and saving for vacations or travel-related expenses.
* `LIFE_PLANS`: Financial planning related to life events, such as weddings, education, or retirement.
* `ENJOYMENT`: Spending on leisure, entertainment, or hobbies.
* `UNSPECIFIED`: No financial topic.
