# Creating & Destroying Objects
## Item 2: Consider using Builder instead of constructor with many parameters
- Alternative 1 - Telescoping Constructor Pattern: You create a constructor for every possible parameter combination. While this pattern works, it's hard to scale and read. Example:
    ```java
    public class NutritionFacts {
        private final int servingSize; // (mL) required
        private final int servings; // (per container) required
        private final int calories; // (per serving) optional
        private final int fat; // (g/serving) optional
        private final int sodium; // (mg/serving) optional
        private final int carbohydrate; // (g/serving) optional
        
        public NutritionFacts(int servingSize, int servings) {
            this(servingSize, servings, 0);
        }
        
        public NutritionFacts(int servingSize, int servings, int calories) {
            this(servingSize, servings, calories, 0);
        }
        
        public NutritionFacts(int servingSize, int servings, int calories, int fat) {
            this(servingSize, servings, calories, fat, 0);
        }
    
        public NutritionFacts(int servingSize, int servings,
            int calories, int fat, int sodium) {
            this(servingSize, servings, calories, fat, sodium, 0);
        }
    
        public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
            this.servingSize = servingSize;
            this.servings = servings;
            this.calories = calories;
            this.fat = fat;
            this.sodium = sodium;
            this.carbohydrate = carbohydrate;
        }
    }
    ```

- Alternative 2 - JavaBeans Pattern: You call a parameterless constructor to create the object and then call setter methods to set each parameter of interest. The problem with this pattern is that there might be inconsistencies in the objects created. There's nothing enforcing the setters to be called. The caller might skip out some setters which might lead to inconsistencies. Also, the class is no longer immutable. Example:
    ```java
    public class NutritionFacts {
        // Parameters initialized to default values (if any)
        private int servingSize = -1; // Required; no default value
        private int servings = -1; // Required; no default value
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public NutritionFacts() { }

        // Setters
        public void setServingSize(int val) { servingSize = val; }
        public void setServings(int val) { servings = val; }
        public void setCalories(int val) { calories = val; }
        public void setFat(int val) { fat = val; }
        public void setSodium(int val) { sodium = val; }
        public void setCarbohydrate(int val) { carbohydrate = val; }
    }
    ```
## Solution - Builder Pattern
```java
// Builder Pattern
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;
    public static class Builder {
        // Required parameters
        private final int servingSize;
        private final int servings;

        // Optional parameters - initialized to default values
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int val) { calories = val; return this; }
        public Builder fat(int val) { fat = val; return this; }
        public Builder sodium(int val) { sodium = val; return this; }
        public Builder carbohydrate(int val) { carbohydrate = val; return this; }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
}
```

1. `NutiritionFacts` class is now immutable (helps in thread safety if I'm not wrong)
2. It's much easier to read and write
3. It exposes a Fluent API
4. The Builder pattern simulates named optional parameters as found in Python and Scala. It's probably not required in Kotlin as it has named parameters
5. For parameter validations, it can be done inside the `Builder` class. It has been skipped in the example above
## Disadvantages
1. In order to create an object, you need to create the builder first. While the cost of doing this is not very noticeable, it might be a problem in performance critical applications
2. It is more verbose than Telescoping Constructor Pattern (so it should only be used if there are many parameters)
