To participate in the MS MARCO Passage Ranking leaderboard, please go here: https://microsoft.github.io/msmarco/Submission.

<!---
# MSMARCO
OFFICIAL WEBPAGE IS [https://microsoft.github.io/msmarco/](https://microsoft.github.io/msmarco/)
A Family of datasets built using technology and Data from Microsoft's Bing.

> MS MARCO: A Human Generated MAchine Reading COmprehension Dataset
> Paper URL : https://arxiv.org/abs/1611.09268

MS MARCO(Microsoft Machine Reading Comprehension) is a large scale dataset focused on machine reading comprehension, question answering, and passage ranking, Keyphrase Extraction, and Conversational Search Studies, or what the community thinks would be useful. 

First released at [NIPS 2016](https://arxiv.org/pdf/1611.09268.pdf), the current dataset has 1,010,916 unique real queries that were generated by sampling and anonymizing Bing usage logs. The dataset started off focusing on QnA but has since evolved to focus on any problem related to search. For task specifics please explore some of the tasks that have been built out of the dataset. If you think there is a relevant task we have missed please open an issue explaining your ideas?  

For more information about [TREC 2019 Deep Learning](https://github.com/microsoft/TREC-2019-Deep-Learning)

For more information about [Q&A](https://github.com/microsoft/MSMARCO-Question-Answering)

For more information about [Ranking](https://github.com/microsoft/MSMARCO-Passage-Ranking)

For more information about [Keyphrase Extraction](https://github.com/microsoft/MSMARCO-OpenKP)

For more information about [Conversational Search](https://github.com/microsoft/MSMARCO-Conversational-Search)

For more information about [Polite Crawling](https://github.com/microsoft/MSMARCO-Optimal-Freshness-Crawl-Under-Politeness-Constraints)


## Dataset Generation, Data Format, And Statistics
What is the difference between MSMARCO and other MRC datasets? We believe the advantages that are special to MSMARCO are:
- Real questions: All questions have been sample from real anonymized bing queries.
- Real Documents: Most Url's that we have source the passages from contain the full web documents. These can be used as extra contextual information to improve systems or be used to compete in our expert task.
- Human Generated Answers: All questions have an answer written by a human. If there was no answer in the passages the judge read they have written 'No Answer Present.'
- Human Generated Well-Formed: Some questions contain extra human evaluation to create well formed answers that could be used by intelligent agents like Cortana, Siri, Google Assistant, and Alexa.
- Dataset Size: At over 1 million queries the dataset is large enough to train the most complex systems and also sample the data for specific applications.

## Download the Dataset
To Download the MSMARCO Dataset please navigate to [msmarco.org](http://www.msmarco.org/dataset.aspx) and agree to our Terms and Conditions. If there is some data you think we are missing and would be useful please open an issue.

# Ranking Task
MS MARCO(Microsoft Machine Reading Comprehension) is a large scale dataset focused on machine reading comprehension, question answering, and passage ranking. A variant of this task will be the part of [TREC](https://trec.nist.gov/) and [AFIRM 2019](http://sigir.org/afirm2019/). For Updates about [TREC 2019](https://github.com/dfcf93/DeepLearningTREC2019) please follow [This Repository](https://github.com/dfcf93/DeepLearningTREC2019)
## Passage Reranking task Task
Given a query q and a the 1000 most relevant passages P = p1, p2, p3,... p1000, as retrieved by BM25 a succeful system is expected to rerank the most relevant passage as high as possible. For this task not all 1000 relevant items have a human labeled relevant passage. Evaluation will be done using [MRR](https://en.wikipedia.org/wiki/Mean_reciprocal_rank)
### Generation
To generate the ranking task dataset we started with the regular MSMARCO dataset which means if people want to generate any data in a different format they are more than able to(and even provide us with suggestions!). We are hoping to open source our production code shortly so people can generate the sets for themselves(with any normalization they may find interesting). 

We collected all unique passages(without any normalization) to make a pool of ~8.8 million unique passages. Then, for each query from the existing MSMARCO splits(train,dev, and eval) we ran a standard [BM25](https://en.wikipedia.org/wiki/Okapi_BM25) to produce 1000 relevant passages. These were ordered by random so each query now has 1000 corresponding passages. Since the original 10 passages presented with the query were extracted using the Bing ranking stack it possible that even none of the original passages are present with this new top 1000.

During the initial dataset creation, the judges would mark any passage that could answer the query which we then translated into our is_selected labels(relevant/used passages have is_selected=1). If a passage had is_selected=1 then this is a relevant query passage pair. It is worth noting that with these labels a positive is a true positive but negatives may not be a true negative(in other words there may be relevant passages with is_selected=0). It is also worth noting that not all 1000 passages were seen by a judge and even ifWhile this means that it is possible for a system to find more relevant passages

To evaluate how well a system is reranking these 1000 relevant passages we use the already existing is_selected flag present in the v2.1 dataset. Given these labels on relevancy, a systems goal should be to rank any of the most relevant passage as high as possible. During the initial dataset creation, the judges would mark any passage that could answer the query which we then translated into our is_selected labels(relevant/used passages have is_selected=1). It is worth noting that with these labels a positive is a true positive but negatives may not be a true negative(in other words there may ne relevant passages with is_selected=0). It is also worth noting that not all 1000 passages were seen by a judge meaning it is possible that there are relevant passages that for purposed of this dataset are not considered relevant. 

Finally, understanding that this ranking data may not be useful to train a Deep Learning style system we build the triples files(availible in small and large(~27 and ~270gb respectively)). These triples contain a query followed by a passage that has been marked as directly relevant(positive example) and another passage that has not been marked as relevant(negative). We understand that there could be a situtation where a one of the negative examples could actually be more relevant than the positive example but given the task goal is to rank the passages where we have a relevance passage as high as possible so this shouldn't be an issue. 

### Data, information, and Formating
Given that all files have been generated from the v2.1 dataset meaning that in theory anyone can generate the files we provide to their own specifications and liking.

| Description                                           | Filename                                                                                                                | File size |                        Num Records | Format                                                         |
|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|----------:|-----------------------------------:|----------------------------------------------------------------|
| Collection                                | [collection.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/collection.tar.gz)                             |    2.9 GB |                         8,841,823  | tsv: pid, passage |
| Queries                                   | [queries.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/queries.tar.gz)                                   |   42.0 MB |                         1,010,916  | tsv: qid, query |
| Qrels Dev                                 | [qrels.dev.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/qrels.dev.tsv)                                     |    1.1 MB |                            59,273  | TREC qrels format |
| Qrels Train                               | [qrels.train.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/qrels.train.tsv)                                 |   10.1 MB |                           532,761  | TREC qrels format |
| Queries, Passages, and Relevance   Labels | [collectionandqueries.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/collectionandqueries.tar.gz)         |    2.9 GB |                        10,406,754  | |
| Train Triples Small                       | [triples.train.small.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/triples.train.small.tar.gz)           |   27.1 GB |                        39,782,779  | tsv: query, positive passage, negative passage |
| Train Triples Large                      | [triples.train.full.tsv.gz](https://msmarco.blob.core.windows.net/msmarcoranking/triples.train.full.tsv.gz)             |  272.2 GB |                       397,756,691  | tsv: query, positive passage, negative passage |
| Train Triples QID PID Format               | [qidpidtriples.train.full.2.tsv.gz](https://msmarco.blob.core.windows.net/msmarcoranking/qidpidtriples.train.full.2.tsv.gz) |    5.7 GB |                       397,768,673  | tsv: qid, positive pid, negative pid |
| Top 1000 Train                            | [top1000.train.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.train.tar.gz)                       |  175.0 GB |                       478,016,942  | tsv: qid, pid, query, passage |
| Top 1000 Dev                              | [top1000.dev.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.dev.tar.gz)                           |    2.4 GB |                         6,669,195  | tsv: qid, pid, query, passage |
#### Full Documents.
The initial MSMARCO dataset included 3.5m urls from which the 8.8m passages were extracted. In March 2018 these urls were scraped and a clean textual representation was created. Due to the time delay many of the domains did not exist and thus we could only product 3.2m documents. These documents can be found in a tsv format that had been cleaned up and optimized for document retrieval and a jsonl format which is mostly for embedding training. The TSV is called []() and format is URL\tTitle\tContent. [TSV](https://msmarco.blob.core.windows.net/msmarcoranking/fulldocs.tsv.gz) and [JSONl](https://msmarco.blob.core.windows.net/msmarcoranking/fulldocuments.jsonl.gz). 
````
erasmus@spacemanidol:~$ head -n 2 fulldocs.tsv
https://answers.yahoo.com/question/index?qid=20071007114826AAwCFvR	The hot glowing surfaces of stars emit energy in the form of electromagnetic radiation.?	Science & Mathematics PhysicsThe hot glowing surfaces of stars emit energy in the form of electromagnetic radiation.?It is a good approximation to assume that the emissivity e is equal to 1 for these surfaces.  Find the radius of the star Rigel, the bright blue star in the constellation Orion that radiates energy at a rate of 2.7 x 10^32 W and has a surface temperature of 11,000 K. Assume that the star is spherical. Use σ =... show moreFollow 3 answersAnswersRelevanceRatingNewestOldestBest Answer: Stefan-Boltzmann law states that the energy flux by radiation is proportional to the forth power of the temperature: q = ε · σ · T^4 The total energy flux at a spherical surface of Radius R is Q = q·π·R² = ε·σ·T^4·π·R² Hence the radius is R = √ ( Q / (ε·σ·T^4·π) ) = √ ( 2.7x10+32 W / (1 · 5.67x10-8W/m²K^4 · (1100K)^4 · π) ) = 3.22x10+13 mSource (s):http://en.wikipedia.org/wiki/Stefan_bolt...schmiso · 1 decade ago0 18 CommentSchmiso, you forgot a 4 in your answer. Your link even says it: L = 4pi (R^2)sigma (T^4). Using L, luminosity, as the energy in this problem, you can find the radius R by doing sqrt (L/ (4pisigma (T^4)). Hope this helps everyone.Caroline · 4 years ago4 1 Comment (Stefan-Boltzmann law) L = 4pi*R^2*sigma*T^4 Solving for R we get: => R = (1/ (2T^2)) * sqrt (L/ (pi*sigma)) Plugging in your values you should get: => R = (1/ (2 (11,000K)^2)) *sqrt ( (2.7*10^32W)/ (pi * (5.67*10^-8 W/m^2K^4))) R = 1.609 * 10^11 m? · 3 years ago0 1 CommentMaybe you would like to learn more about one of these?Want to build a free website? Interested in dating sites?Need a Home Security Safe? How to order contacts online?
http://childparenting.about.com/od/physicalemotionalgrowth/tp/Child-Development-Your-Eight-Year-Old-Child.htm	Developmental Milestones and Your 8-Year-Old Child	"School-Age Kids Growth & DevelopmentDevelopmental Milestones and Your 8-Year-Old Child8-Year-Olds Are Expanding Their WorldsBy Katherine Lee | Reviewed by Joel Forman, MDUpdated February 10, 2018Share Pin EmailPrintEight-year-olds are becoming more confident about themselves and who they are. At age 8, your child will likely have developed some interests and hobbies and will know what he or she likes or doesn't like.At the same time, children this age are learning more about the world at large and are also better able to navigate social relationships with others more independently, with less guidance from parents. At home, 8-year-olds are able to tackle more complicated household chores and take on more responsibility for taking care of themselves, even helping out with younger siblings.In general, according to the CDC, these are some changes you may see in your child:Shows more independence from parents and family.Starts to think about the future.Understands more about his or her place in the world.Pays more attention to friendships and teamwork.Wants to be liked and accepted by friends.1 Behavior and Daily RoutinesFabrice LeRouge/Getty ImagesThe 8-year-old's behavior and daily routines are shaped by the child's taste, interests, and personality. Parents and other significant adults in the child's life should keep in mind the importance of being good role models since this is a time when children are figuring out the world and who they are and how they fit into it. At this age, your child may get involved with more complex social activities and behaviors that help define his or her sense of self.Effective discipline techniques at this age include continuing to praise good behavior, focusing your child's efforts, what they can do and change, rather than innate traits (such as ""you are smart""). Set up and enforce consistent rules. Discipline should be aimed at guiding your child rather than punishing. Follow it with a discussion with your child about what she could do differently next time.Your 8-year-old can do more self-care in regards to hygiene and may begin to want to be part of deciding what the family eats. You might begin to give your child chores to contribute to the maintenance of the household and an allowance to begin to learn to manage money. At this age, your child still needs 10 to 11 hours of sleep per night.2 Physical DevelopmentImage Source/Getty ImagesFor 8-year-old children, physical development will continue to be more about refinement of skills, coordination, and muscle control rather than huge changes. They begin to look like ""big kids,"" but puberty is still a couple of years away for most of them.Children with natural athletic potential may show their abilities at this developmental stage as their physical skills become more precise and accurate. In fact, this is often the age at which children decide whether they are athletic or not, and choose to participate in or avoid sports. Either way, it's important for parents to encourage physical activity. Even if your child isn't an athlete she can still enjoy running, swimming, biking, and many other types of non-sports-related physical fun.3 Emotional DevelopmentJohn Howard/Getty ImagesEight-year-old emotional development may be growing at a deeper level than in younger years, and an 8-year-old may show more sophisticated and complex emotions and interactions. For instance, an 8-year-old may mask true thoughts or emotions to spare someone else's feelings or work through a problem without an adult's close supervision or intervention.This is the time when your child may be developing a more sophisticated sense of himself in the world. Her interests, talents, friends, and relationship with family all help her to establish a clear self-identity. It's also the beginning of desiring privacy and flip-flopping between self-confidence and self-doubt.It can be a good time to help your child develop patience and empathy for others.4 Cognitive DevelopmentTom Merton/Getty ImagesEight-year-old children are at a stage of intellectual development where they will be able to pay attention for longer periods of time. You can expect your child to be able to concentrate on an activity for up to an hour or more. Eight-year-olds will also be able to think more critically and express opinions using more complex and sophisticated vocabulary and language skills.5 Social DevelopmentChristopher Futcher/Getty ImagesThis is the phase of social development where many children love being a part of sports teams and other social groups. In general, 8-year-old children enjoy school and will count on and value relationships with a few close friends and classmates. Parents of 8-year-olds should be on the lookout for problems such as school refusal, as this may indicate learning difficulties or being bullied at school. It is also a good age at which to discuss respecting others.6 What If My Child Is Different?Developmental milestones provide professionals and parents with a tool for comparing children to a norm. No child fits the ideal norm perfectly, and each child will have his or her personal quirks, strengths, challenges, and preferences. With that said, however, if you feel your child is far behind or ahead of the norm, it's well worth discussing the issue with your pediatrician and your child's teacher. If there are issues or opportunities, now is the time to learn about and address them.A Word From VerywellYour 8-year-old is in the full bloom of childhood. Enjoy activities and explore the world together. It's a great time to spark new interests in your child and watch her grow in every way.Sources:Anthony, Michelle. The emotional lives of 8-10-year-olds. Scholastic Publishing.Chaplin TM, Aldao A. Gender differences in emotion expression in children: A meta-analytic review Psychological Bulletin. 2013;139 (4):735-765. doi:10.1037/a0030737.Middle childhood. CDC."
````




#### Passage to PassageID
This file contains each unique Passage in the larger MSMARCO dataset. Format is PID\tPassage and the file is collection.tsv. It can be found [Here](https://msmarco.blob.core.windows.net/msmarcoranking/collectionandqueries.tar.gz)
````
7	Manhattan Project. The Manhattan Project was a research and development undertaking during World War II that produced the first nuclear weapons. It was led by the United States with the support of the United Kingdom and Canada. From 1942 to 1946, the project was under the direction of Major General Leslie Groves of the U.S. Army Corps of Engineers. Nuclear physicist Robert Oppenheimer was the director of the Los Alamos Laboratory that designed the actual bombs. The Army component of the project was designated the
8	In June 1942, the United States Army Corps of Engineersbegan the Manhattan Project- The secret name for the 2 atomic bombs.
9	One of the main reasons Hanford was selected as a site for the Manhattan Project's B Reactor was its proximity to the Columbia River, the largest river flowing into the Pacific Ocean from the North American coast.
````
Size info
````
8841823 collection.tsv
````
#### Query to QueryID
This has been split for Train, Dev and Eval. These sets include all queries including those which do not have answers. If queries with no answer were removed the sets would be around 35% smaller. To avoid having extremely large submissions we have subset the dev and eval portions to be ~1/8 of the QnA set. In other words the dev and eval set are ~6800 queries instead of the full 55,000. To find only the queries associated with the public sets use the queries.dev.small.tsv and queries.eval.small.tsv. There are 5 files and they can be found inside [Here](https://msmarco.blob.core.windows.net/msmarcoranking/collectionandqueries.tar.gz)
````
121352	define extreme
634306	what does chattel mean on credit history
920825	what was the great leap forward brainly
510633	tattoo fixers how much does it cost
737889	what is decentralization process.
278900	how many cars enter the la jolla concours d' elegance?
674172	what is a bank transit number
303205	how much can i contribute to nondeductible ira
570009	what are the four major groups of elements
492875	sanitizer temperature
````
Size info
````
  101093 queries.dev.tsv
  101092 queries.eval.tsv
  808731 queries.train.tsv
 1010916 total
````
#### Top1000
These files are split between train, dev, and eval. For each query there are ~1000 passages which were retrived by BM25 from the 8.8m collection. The train set contains all examples(~550,000 queries) but to make evaluation faster we have segmented the dev and eval file to be 1/8 of the full size. In other words, dev and eval are ~6800 queries out of the 55000 possible. [Dev](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.dev.tar.gz),[Eval](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.eval.tar.gz), and [Train](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.train.tar.gz)
````
188714  1000052 foods and supplements to lower blood sugar      Watch portion sizes: â  Even healthy foods will cause high blood sugar if you eat too much. â  Make sure each of your meals has the same amount of CHOs. Avoid foods high in sugar: â  Some foods to avoid: sugar, honey, candies, syrup, cakes, cookies, regular soda and.
1082792 1000084 what does the golgi apparatus do to the proteins and lipids once they arrive ?  Start studying Bonding, Carbs, Proteins, Lipids. Learn vocabulary, terms, and more with flashcards, games, and other study tools.
995526  1000094 where is the federal penitentiary in ind        It takes THOUSANDS of Macy's associates to bring the MAGIC of MACY'S to LIFE! Our associate team is an invaluable part of who we are and what we do. F ind the seasonal job that's right for you at holiday.macysJOBS.com!
199776  1000115 health benefits of eating vegetarian    The good news is that you will discover what goes into action spurs narrowing of these foods not only a theoretical supposition there are diagnosed with great remedy is said that most people and more can be done. Duncan was a wonderful can eating chicken cause gout benefits of natural. options with your health.
660957  1000115 what foods are good if you have gout?   The good news is that you will discover what goes into action spurs narrowing of these foods not only a theoretical supposition there are diagnosed with great remedy is said that most people and more can be done. Duncan was a wonderful can eating chicken cause gout benefits of natural. options with your health.
820267  1000130 what is the endocrine system responsible for?   The pancreas secretes pancreatic enzyme which is responsible for the breakdown of protein but it also secretes insulin so that we can get energy from glucose making it a part of the endocrine system or glands. The liver de-toxifies all of the blood that carries absorbed nutrients from the digestive system.
837202  1000252 what is the nutritional value of oatmeal        Oats make an easy, balanced breakfast. One cup of cooked oatmeal contains about 150 calories, four grams of fiber (about half soluble and half insoluble), and six grams of protein. To boost protein further, my favorite way to eat oatmeal is with a swirl of almond butter nestled within.
130825  1000268 definition for daring   Such a requirement would have three desirable consequences: First, it would tend to make bank executives more conservative and less daring in gambling with other people's money; second, it would put this liability of financial decision makers ahead of any taxpayer bailout in case of insolvency; and third, it would create a potentially powerful diseconomy of scale within big conglomerate banks.
408149  1000288 is dhgate a scam        If you think you ve been targeted by a counterfeit check scam, report it to the following agencies: 1  The Federal Trade Commission or 1-877-FTC-HELP (1-877-382-4357). 2  The U.S. Postal Inspection Service or call your local post office. 3  The number is in the Blue Pages of your local telephone directory.ere s how to avoid a counterfeit check scam: 1  Throw away any offer that asks you to pay for a prize or a gift. 2  If it s free or a gift, a promotion or a sweepstakes, you shouldn t have to pay anything for it.
345453  1000327 how to become a teacher assistant       Top 10 amazing movie makeup transformations. Biological chemistry, or biochemistry, is the study of the chemical composition of living organisms at a cellular level.omeone seeking a career in biological chemistry will usually need at least a bachelorâs degree. With a bachelorâs degree, an individual can qualify for a job as a science teacher at the high school level, a research assistant, laboratory technician, or a scientist in a testing environment.
````
Size info
````
   6668967 top1000.dev.tsv
   6515736 top1000.eval.tsv
  13184703 total
````
#### Relevant Passages
We have processed the train and dev set and made a QID to PID mapping of when a question has had a passage marked as relevant. We have held out the eval set but its distribution matches that of dev. As mentioned above, since since top1000.dev and top1000.eval are samples there exists qrels.dev.tsv(full qrels on 55,000 queries) and qrels.dev.small.tsv(which are the qrels corresponding to all queries in top1000.dev). Column 0 is queryID, column 2 is passageID. Please ignore columns 1 and 3(They are there for TREC formating). There are 5 files and they can be found inside [Here](https://msmarco.blob.core.windows.net/msmarcoranking/collectionandqueries.tar.gz)
````
1185869 0       0       1
1185868 0       16      1
597651  0       49      1
403613  0       60      1
1183785 0       389     1
312651  0       616     1
80385   0       723     1
645590  0       944     1
645337  0       1054    1
186154  0       1160    1
````
Size info
````
    7437 qrels.dev.small.tsv
   59273 qrels.dev.tsv
    7304 qrels.eval.small.tsv
   59187 qrels.eval.tsv
  532761 qrels.train.tsv
  665962 total
````
#### Triples.Train
The `triples.train.<size>.tsv` are two files that we have created as an easy to consume training dataset. Each line of the TSV contains querytext, A relevant passage, and an non-relevant passage all separated by `\t`. The only difference between triples.train.full.tsv and triples.train.small.tsv is the smaller is ~10% of the overall size since the full sized train is > 270gbs. [Full Size](https://msmarco.blob.core.windows.net/msmarcoranking/triples.train.full.tsv.gz) and [Small](https://msmarco.blob.core.windows.net/msmarcoranking/triples.train.small.tar.gz)

Example line:
````
what fruit is native to australia       Passiflora herbertiana. A rare passion fruit native to Australia. Fruits are green-skinned, white fleshed, with an unknown edible rating. Some sources list the fruit as edible, sweet and tasty, while others list the fruits as being bitter and inedible.assiflora herbertiana. A rare passion fruit native to Australia. Fruits are green-skinned, white fleshed, with an unknown edible rating. Some sources list the fruit as edible, sweet and tasty, while others list the fruits as being bitter and inedible.   The kola nut is the fruit of the kola tree, a genus (Cola) of trees that are native to the tropical rainforests of Africa.
````

### Evaluation
Evaluation of systems will be done using MRR@10. We have selected such a low MRR number because the sizes of files candidates need to create quickly balloon with each additional depth. Official evaluation scripts is [Here](https://github.com/dfcf93/MSMARCOV2/blob/master/Ranking/Baselines/msmarco_eval.py).

Expected format for your submission is a file including qid\tpid\trank for each query/top 1000 in the eval file on the website. To minimize space feel free to only inclde the top 100 or 10 passages.
````
1124703 8766037 1
1124703 8021997 2
1124703 7816201 3
1124703 8296123 4
1124703 8790898 5
1124703 5451590 6
1124703 8021999 7
1124703 8388210 8
1124703 8702520 9
1124703 8790903 10
````
#### Rules
Since the Passage Reranking dataset is based on the original MSMARCO dataset it is possible to use some of the exisiting ranking signals in the original dataset as a relevance signal. In other words people can leverage the connection between the query and the 10 Bing passages in the original dataset and could be used to promote those passages or mine them for query expansion terms (relevance feedback). To prevent confusion of model performance we as any team that uses any signals from the initial dataset to describe what they used and we will mark the run as special. In addition, if you use any outside signal(or internal signal) that you think we should know and make know to the larger community please include a description in your submision. 

### Submissions
Once you have built a model that meets your expectations on evaluation with the dev set, you can submit your test results to get official evaluation on the test set. To ensure the integrity of the official test results, we do not release the correct answers for test set to the public. To submit your model for official evaluation on the test set, follow the below steps:
Generate your proposed reranking for the Top1000 passages for the Eval and the Dev set. To encourage reproducibility of results we encourage all teams to submit their code along with documentation and hyperparameters used.
Submit the following information by [contacting us](mailto:ms-marco@microsoft.com?subject=MS%20Marco%20Submission)
* Individual/Team Name: Name of the individual or the team to appear in the leaderboard [Required]
* Individual/Team Institution: Name of the institution of the individual or the team to appear in the leaderboard [Optional]
* Model information: Name of the model/technique to appear in the leaderboard [Required]
* Paper Information: Name, Citation, URL of the paper if model is from a published work to appear in the leaderboard [Optional]
* Code Information: A github repo of your model, instruction of how to use, etc [Optional]

To avoid "P-hacking" we limit teams/individuals to 1 per week and we will update the leaderboard to include all submisions by such teams, not just the most recent. 

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
-->
