
## TakeMaker
a fine-tuned text classifier that evaluates discourse quality in r/TrueFilm


## Recommended Stack

| Component | Tool | Notes |
|-----------|------|-------|
| Base model | `distilbert-base-uncased` | Hugging Face — free to download, no account needed |
| Fine-tuning | Google Colab (free GPU) | Free T4 GPU; fine-tuning DistilBERT on 200 examples takes ~5–15 min |
| Training libraries | `transformers` + `datasets` + `scikit-learn` | Pre-installed on Colab — no setup needed |
| Baseline LLM | Groq (`llama-3.3-70b-versatile`) | Free tier — same account as Projects 1–2 |


## Community
 r/TrueFilm is one of Reddit's largest subrredit for cinema heads, enthusiasts and critques 

TakeMeter labels posts as:
> **reaction:** an emotional response to a film, driven by personal feeling and taste rather than in depth analytical reasoning.

> **critique:** analysis, interpretation, and evaluation of a movie as an art form — examining its technical execution, narrative structure, thematic meaning, or cultural impact through observation, interpretation, and reasoned argument. May include reaction but mainly an analysis

> **suggestion request:** a request for film recommendations, usually framed around a mood, genre, director. May include critique or reaction but mainly a request

These labels matter to this community because the members of TrueFilm believe members in this group care a lot about film and a distinction between "I loved this film" and "This film is good because it's why this film's editing undermines its thesis" is exactly the line members care about. TakeMeter will help surface which kind of contribution a post actually is.

## Examles**
**reaction:** 
> https://www.reddit.com/r/TrueFilm/comments/1udjddx/i_finally_watched_obsession/
> https://www.reddit.com/r/TrueFilm/comments/1uboyo9/hirokazu_koreeda/


**critiques:** 
> https://www.reddit.com/r/TrueFilm/comments/1uektjo/obsession_is_about_ai_and_i_think_this_reading/
> https://www.reddit.com/r/TrueFilm/comments/1ucyku8/obsession_male_passivity/

**suggestion requests:** 
> https://www.reddit.com/r/TrueFilm/comments/1f2xjfq/looking_for_suggestions/
> https://www.reddit.com/r/TrueFilm/comments/1ucyku8/obsession_male_passivity/

---
## Hard Edge Cases and Solutions
Some of r/TrueFilm's posts are written in a hedging register with titles like "am I wrong about this?" but turn out to actually be critiques. This is because people are not professional critiiques so they frame their critiques as reactions to invite discussion. TakeMeter may misclassify these as reactions based on surface level phrasing. To solve this i will not include the misleading post titles in the post
> e.g https://www.reddit.com/r/TrueFilm/comments/1uejyzg/am_i_the_only_one_who_thinks_obsession_completely/


This feels like a reaction but the questions sound like critiques so i'm not including it so the model is not confused
> https://www.reddit.com/r/TrueFilm/comments/1ueqw53/i_love_the_neon_demon/


Other hard cases a posts that are both. They start out as a reaction but then also critique. To solve this i believe a reaction that has critques is aslo critique but a critique that includes a reaction is simply a reaction so I will label accordingly 
> e.g https://www.reddit.com/r/TrueFilm/comments/1udggw5/obsession_who_are_the_victims/



---

## Data Collection Plan

Where will you collect examples? How many per label? What will you do if a label is underrepresented after 200 examples?

> I will collect samples from r/TrueFilm's posts with a ~50/50 split per label. If underreppresented i will add more in the underrepresented label

## Evaluation Metrics

Which metrics will you use to evaluate your model and why are those the right ones for this specific task? Accuracy alone is not enough — explain what else you need and why.
> Accuracy to see what fraction the classifier got right
> error analysis to examine which examples the classifier gets wrong and understand what pattern the model captured and what it missed.

## Definition of Success**

What performance would make this classifier genuinely useful? What would you accept as "good enough" for deployment in a real community tool?

> 89% accuracy is good enough for deployment 

## AI Tool Plan 
>Label stress-testing: I'll ask claude my label definitions and edge case description, and ask it to generate 5–10 posts that sit at the boundary between two labels.

>Annotation assistance: I'll use claude to pre-label a batch of examples before reviewing them myself and add a file to not pre-labeled examples.

>Failure analysis: If i have more than 20& wrong predictions i plan to give a list of them to claude and ask it to identify patterns before you write up me evaluation. I will read through the patterns and examples to compare and determine if the pattern is plausible based on the inputs
# Label Distribution
Total examples: 155

Label distribution:
label
critique              68
reaction              59
suggestion request    28
Name: count, dtype: int64

# Prompt 
SYSTEM_PROMPT = """
You are classifying comments and posts from  r/TrueFilm .
Assign each post to exactly one of the following categories.

suggestion request: a request for film recommendations, usually framed around a mood, genre, director whuch include critique or reaction but is mainly a request
Example: "I don’t know what my favorite genres are and I can’t decide what to watch next. I will list my top 10 for the reference.
Clockwork Orange, Y Tu Mama Tambien, High and Low, Stray Dog, The Third Man, Oppenheimer, About Elly, Maudie, Boyhood, Showing Up.
Honorable mentions will be Nickel Boys, The Lives of Others, Running on Empty. I want to have Farewell My Concubine on but MC’s love interest was such an unlikable person (was not the case in the novel).
I am Japanese, Japan resident, mid 20s. I know I am still a casual but I want to watch more movies. I will see replies at night! Thank you!"

critique: analysis, interpretation, and evaluation of a movie as an art form — examining its technical execution, narrative structure, thematic meaning, or cultural impact through observation, interpretation, and reasoned argument.
Example: "The scene, rendered for the most part in a long shot so all the signifiers are equally present, is a perfect icon of Coca-Cola imperialism. Both the pros­titute and the Asian face are grotesque parodies of materialist American culture reminiscent of the Dr. T. J. Eckleburg billboard that overlooks the wasteland of the Valley of Ashes in F. Scott Fitzgerald’s novel about the American Dream, The Great Gatsby. Kubrick’s juxtaposition signifies that Third World and colonized people can only be conceptualized by the colo­nizer insofar as they attempt to assimilate his culture (in this case by becoming commodities). They are only valued insofar as they accommodate to imperi­alist prerogatives. As a marine commanding officer says: “We are here to help the Vietnamese because inside every gook there is an American.’’ The boots in the song signify America’s presence in Vietnam, walking all over the coun­try and the culture. As the scene draws to a close the screen fades to black and in the darkness the concluding words of the chorus are repeated by the female voice of the singer: “These boots are gonna walk all over you.” It is an indica­tion—almost subliminal—that some sort of reversal is going to take place."

reaction: an emotional response to a film, driven by personal feeling and taste rather than in depth analytical reasoning.
Example: "A friend had recommended it to me in really high regards, as well as all of Powell & Pressburger's work, so I was excited to check this out, and had high expectations. In the first hour, those expectations were exceeded without a doubt. Then, the second hour hit. Man, what a trainwreck. 15 minutes is spent trying to communicate equality between the British and Americans. I really wish I could've loved this film, I really loved the first hour and saw just how influential it was, but my god is the second hour a difficult watch."

Respond with ONLY the label name.
Do not explain your reasoning.

Valid labels:
suggestion request
critique
reaction
"""

# Baseline Approach
> overal 71 perent accuracy 
> with first prompt :
**results**
Per-class metrics (baseline):
                    precision    recall  f1-score   support

          critique       1.00      0.45      0.62        11
          reaction       0.57      0.89      0.70         9
suggestion request       0.80      1.00      0.89         4

          accuracy                           0.71        24
         macro avg       0.79      0.78      0.74        24
      weighted avg       0.81      0.71      0.70        24