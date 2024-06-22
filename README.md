Code:
library(tidyverse)
library(syuzhet)
library(tidytext)
# import text dataset
getwd()
setwd("~/aaa")
df = read.csv("restaurants_reviews 3.csv")
view(df)
text.df <- tibble(text = str_to_lower(df$reviews_list))
text.df
# analyze sentiments using the syuzhet package based on the NRC sentiment dictionary
emotions <- get_nrc_sentiment(text.df$text)
View(emotions)
emo_bar <- colSums(emotions)
emo_sum <- data.frame(count=emo_bar, emotion=names(emo_bar))
emo_bar
# create a barplot showing the counts for each of eight different emotions and positive/negative rating
ggplot(emo_sum, aes(x = reorder(emotion, -count), y = count)) + 
 geom_bar(stat = 'identity') 
# sentiment analysis with the tidytext package using the "bing" lexicon
bing_word_counts <- text.df %>% unnest_tokens(output = word, input = text) %>%
 inner_join(get_sentiments("bing")) %>%
 count(word, sentiment, sort = TRUE) 
# select top 10 words by sentiment
bing_top_10_words_by_sentiment <- bing_word_counts %>% 
 group_by(sentiment) %>% 
20
 slice_max(order_by = n, n = 10) %>% 
 ungroup() %>% 
 mutate(word = reorder(word, n)) 
bing_top_10_words_by_sentiment
# create a barplot showing contribution of words to sentiment
bing_top_10_words_by_sentiment %>% 
 ggplot(aes(word, n, fill = sentiment)) + 
 geom_col(show.legend = FALSE) + 
 facet_wrap(~sentiment, scales = "free_y") + 
 labs(y = "Contribution to sentiment", x = NULL) + 
 coord_flip() 
# sentiment analysis with the tidytext package using the "loughran" lexicon
loughran_word_counts <- text.df %>% unnest_tokens(output = word, input = text) %>%
 inner_join(get_sentiments("loughran")) %>%
 count(word, sentiment, sort = TRUE) 
# select top 10 words by sentiment
loughran_top_10_words_by_sentiment <- loughran_word_counts %>% 
 group_by(sentiment) %>% 
 slice_max(order_by = n, n = 10) %>% 
 ungroup() %>%
 mutate(word = reorder(word, n))
loughran_top_10_words_by_sentiment
# create a barplot showing contribution of words to sentiment
loughran_top_10_words_by_sentiment %>% 
 ggplot(aes(word, n, fill = sentiment)) + 
 geom_col(show.legend = FALSE) + 
 facet_wrap(~sentiment, scales = "free_y") + 
 labs(y = "Contribution to sentiment", x = NULL) + 
 coord_flip()

 .................................
 explanation:

 Certainly! Here's a simplified explanation of how to put your R code and associated files on GitHub:

### Step-by-Step Explanation:

1. **Create a GitHub Repository:**
   - Sign in to GitHub and create a new repository by clicking on the **"+"** sign next to your profile picture and selecting **"New repository"**.
   - Choose a name for your repository (e.g., `restaurant_reviews_analysis`) and optionally add a description. Decide if you want it to be public or private.
   - Initialize the repository with a README file, which can provide an overview of your project.

2. **Upload Your Files:**
   - Clone (download) the repository to your local machine using Git. You can do this by opening Git Bash or another terminal and using the command:
     ```
     git clone https://github.com/your-username/restaurant_reviews_analysis.git
     ```
     Replace `your-username` with your actual GitHub username.
   - Place your R script (`analysis_script.R`), CSV data (`restaurants_reviews 3.csv`), and any other relevant files into this local repository folder.

3. **Commit and Push Changes:**
   - Navigate to the directory where you cloned the repository using the terminal:
     ```
     cd restaurant_reviews_analysis
     ```
   - Add all the files to the repository:
     ```
     git add .
     ```
   - Commit the files with a descriptive message:
     ```
     git commit -m "Add R script and data for sentiment analysis"
     ```
   - Push these changes to GitHub:
     ```
     git push origin main
     ```
     This command pushes your local changes to the main branch of your GitHub repository (`main` assuming it's the default branch name).

4. **README (Optional):**
   - Update the `README.md` file in your GitHub repository to provide essential information about your project. Include details such as the purpose of your analysis, instructions for running the R script, and any prerequisites.

5. **Share Your Repository:**
   - Once your files are uploaded and everything is set, you can share the link to your GitHub repository (`https://github.com/your-username/restaurant_reviews_analysis`). This allows others to view your analysis, provide feedback, or collaborate on further developments.

