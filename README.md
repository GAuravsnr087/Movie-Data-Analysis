# Netflix Movie Data Analysis Project

## Overview
This project analyzes a dataset of Netflix movies to uncover patterns and insights. By exploring genres, release trends, viewer ratings, and popularity, we aim to understand Netflix's movie catalog better.

## Dataset Summary
- The dataset consists of **9,827 rows** and **9 columns**.
- It is tidy, with no missing or duplicate values.
- Columns `Overview`, `Original_Language`, and `Poster_Url` are dropped as they are not useful for analysis.
- The `Release_Date` column is converted to year format for better analysis.
- The `Vote_Average` column is categorized into 4 categories: **popular, average, below_avg, not_popular**.
- The `Genre` column is cleaned and exploded to have one genre per row.


## Data Preprocessing
### Steps:
1. **Convert `Release_Date` to Year:**
   ```python
   df['Release_Date'] = pd.to_datetime(df['Release_Date']).dt.year
   ```

2. **Drop Unwanted Columns:**
   ```python
   cols = ['Overview', 'Original_Language', 'Poster_Url']
   df.drop(cols, axis=1, inplace=True)
   ```

3. **Categorize `Vote_Average`:**
   ```python
   def categorize_col(df, col, labels):
       edges = [df[col].describe()['min'],
                df[col].describe()['25%'],
                df[col].describe()['50%'],
                df[col].describe()['75%'],
                df[col].describe()['max']]
       df[col] = pd.cut(df[col], edges, labels=labels, duplicates='drop')
       return df

   labels = ['not_popular', 'below_avg', 'average', 'popular']
   categorize_col(df, 'Vote_Average', labels)
   ```

4. **Clean and Explode `Genre`:**
   ```python
   df['Genre'] = df['Genre'].str.split(', ')
   df = df.explode('Genre').reset_index(drop=True)
   ```

## Data Visualization
1. **Most Frequent Genre:**
   ```python
   sns.catplot(y='Genre', data=df, kind='count',
               order=df['Genre'].value_counts().index, color='#4287f5')
   plt.title('Genre Column Distribution')
   plt.show()
   ```

2. **Vote Distribution:**
   ```python
   sns.catplot(y='Vote_Average', data=df, kind='count',
               order=df['Vote_Average'].value_counts().index, color='#4287f5')
   plt.title('Vote Distribution')
   plt.show()
   ```

3. **Year-wise Movie Releases:**
   ```python
   df['Release_Date'].hist()
   plt.title('Year Wise Movie Released')
   plt.show()
   ```

## Key Insights
1. **Most Frequent Genre:**
   - Drama is the most frequent genre, appearing in over 14% of the dataset.

2. **Genres with Highest Votes:**
   - 25.5% of the dataset falls under the "popular" category, with Drama leading by over 18.5%.

3. **Movie with Highest Popularity:**
   - *Spider-Man: No Way Home* is the most popular movie, categorized under Action, Adventure, and Science Fiction.

4. **Movie with Lowest Popularity:**
   - *The United States, Thread* has the lowest popularity, with genres like Music, Drama, War, Sci-Fi, and History.

5. **Year with Most Movies Released:**
   - The year 2020 had the highest number of movie releases in the dataset.

## Conclusion
This analysis provides valuable insights into Netflix's movie catalog, highlighting trends in genres, popularity, and release patterns. The findings can assist in making data-driven decisions for content creation and user engagement.

## Contact
For any queries or contributions, feel free to reach out:
- **Email:** grvsnr087@gmail.com

