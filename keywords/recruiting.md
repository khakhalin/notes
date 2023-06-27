# Recruiting, hiring, interviewing

See also: 
* [[job_search]] - kind of the opposite perspective
* [[system_interview]] - how to prepare for systems interivews
* [[behav_interview]] - how to prepare for behav interviews

Related:
* [[id20201020160912]] - why CS recruiting market is broken, Aline Lerner 2020
* [[Saroufim2021screening]] - that objective tech interviews are impossible, Mark Saroufim 2021
* [[Bock2015workrules]] - a book about Google corporate culture

#management #bib #interview


General ideas for good interviewing:
* Don't do unstructured interviews (they just reinforce your biases). Always come with a structure first, a list of questions to ask, ask all candidates these questions, and score them in the same way
* Questions that don't help (don't correlate with performance): trivia, puzzles, invasive behavioral questions. Eliminate them.
* Questions that help: anything that resembles real work. So either questions about past work (asking them to explain something they did - tests both what they did, and how well they can explain it), or questions about potential problems they may face in their new job (like, how would you approach this problem, or that situation)
* Actual mini-projects (2-3 hours) work even better, but may be too expensive in practice

# Good practices

Erik Bernhardsson. How to hire smarter than the market: a toy model. 2020.
https://erikbern.com/2020/01/13/how-to-hire-smarter-than-the-market-a-toy-model.html
1. In any competitive pool, any 2 talents will anti-correlate, because of saturation of resources: because people develop either skill 1 or skill 2, and their time / resources are finate. For a given age, it's a zero-sum game.
2. This also goes experience and performance. Selecting for more experienced people means that you are anti-selecting for performant people, and vice versa.
3. In the extreme case of going for "most rounded candidates" we actually actively select subpar performance, just by the nature of our selection. 
4. It means that the best strategy to win over a competitive market is to officially give up on some of the "virtues" in your candidates. It allows to get better candidates on all other dimensions. This approach is especially helpful if you think in terms of balanced teams, rather than individual candidates.  In a balanced "pokemon teams", people with extreme talents will support and enrich each other.
See also (Wee 2014) below, for a similar argument in a context of ability testing.

Aline Lerner (2023) What do the best interviewers have in common? We looked at thousands of real interviews to find out. https://interviewing.io/blog/best-technical-interviews-common
Analyzed feedbacks to thousands of real interviews (not practice ones) on their platform, and checked whether the candidates were willing to take a 2nd interview with a company after they passed the 1st one (corrected by candidate overall score!) The quality of a question mattered a lot (unlike the company name!).
* Good questions don't look like puzzles (trivia), but like real-life tasks in miniature that can be explored (edge cases, layers of depth) and complicated as you go. Extra bonus if they have a real-life connection to the company. 
* On the other extreme, trying to guess what the interviewer is even asking, questions with one narrow solution, and narrow domain-specific questions, all make for bad experiences.
* Providing helpful hints, and generally simulating (creating) a collaborative collegial experience is even more important. The feelings of being judged or ignored however are bad.

# Bad practices

Ammon Bartram. 2015. Who Y Combinator companies want.
They asked ~30 big software companies to fill a table about what properties of job candidates they value, and which ones actually hurt an application. They found that **there is almost no correlation between companies, in terms of how they define a good candidate!** It is completely unpredictable, from company to company, and seems to often follow the profile of the founder of each company. The only few (weak and obvious) universals that they found: 
* Experience matters (duh)
* Everyone likes product (result) -oriented people, as opposed to technique-based people;
* Everyone likes practical programmers, and dislikes academic programmers.
* Most people dislike programmers with large corporate experience, and prefer programmers with small-edgy-hackery experience (in 2015 sotware development it was about disliking C# and Java, and preferring Ruby and JS, but I bet it changed since)
Refs:
* Original link (as of 2023, broken): https://triplebyte.com/blog/who-y-combinator-companies-want
* Webarchive link: https://web.archive.org/web/20230327040331/https://triplebyte.com/blog/who-y-combinator-companies-want
* Tweet to quote: https://twitter.com/DynamicWebPaige/status/1377464761013653505
Thoughts:
* This kinda suggests that unless you are truly desperate, it's better not to tailor your resume / cover letter to every company, but to target it to the type of companies that you like. Then companies that you would have hated will reject you (which is good), while good companies would keep you in the pool.

Highhouse, S. (2008). **Stubborn reliance on intuition and subjectivity in employee selection**. Industrial and Organizational Psychology, 1(3), 333-342
https://www.guruji24.com/acwi_guru/resources/blog_files/6Organizational_Psychology_-_Reliance.pdf
* Anti-test sentiment and a desire for "holistic assessment" are ruining selection and hiring, as they introduce random noise in decision-making. And yet people keep to stubbornly believe in their ability to guess who the candidates "really are". Summarizes hard data that adding personal assessment to test scores reduces quality of prediction about the success of students, candidates. Moreover, intuition of a recruiter doesn't improve with experience (many refs here). People believe that they get better in finding good candidates, but they actually aren't getting better; they are just getting more confident.
* The only counter-argument against fully mechanistic selection is the "broken-leg factor": human can incorporate "out-of-distribution" unexpected information in ways that rubrics and pre-trained models can't (the name comes from an example: a person with a freshly broken leg is not likely to go to a cinema, which has nothing to do with whether they like the movie, and that a model trained on personal tastes will be likely to miss).

Dalal, D. K., Sassaman, L., & Zhu, X. S. (2020). The impact of nondiagnostic information on selection decision making: A cautionary note and mitigation strategies. Personnel Assessment and Decisions, 6(2), 7.
https://scholarworks.bgsu.edu/cgi/viewcontent.cgi?article=1117&context=pad
About biases in hiring (they call it "nondiagnostic information"). "Stubborn reliance on intuition" as the main enemy of good hiring processes (see Highhouse 2008).

Meijer, R. R., Neumann, M., Hemker, B. T., & Niessen, A. S. M. (2020). A tutorial on mechanical decision-making for personnel and educational selection. Frontiers in psychology, 10, 3002.
https://www.frontiersin.org/articles/10.3389/fpsyg.2019.03002/full
In hiring, more information is not better. Even completely noisy measures will be given some weight by people (that's just how our brains work), if they are provided, making hiring outcomes objectively worse. As in ML, noisy features can make the model worse, so it's better not to try to be holystic, but decide what matters in a good candidate, and only intenitonally measure / assess / probe these things.
* Footnote: citation for that: Adams, G.S., Converse, B.A., Hales, A.H. et al. People systematically overlook subtractive changes. Nature 592, 258–261 (2021). https://doi.org/10.1038/s41586-021-03380-y

Zhang, D. C. (2021). Horse-sized ducks or duck-sized horses? Oddball Personality Questions are likable (but useless) for organizational recruitment. Journal of Business and Psychology, 1-19. https://psyarxiv.com/phnsr/

Kausel, E. E., Culbertson, S. S., & Madrid, H. P. (2016). Overconfidence in personnel selection: When and why unstructured interview information can hurt hiring decisions. Organizational Behavior and Human Decision Processes, 137, 27-44.
Unstructured interviews not just don't help, but actually hurt the quality of hiring.

Woolley, K., & Fishbach, A. (2018). Underestimating the importance of expressing intrinsic motivation in job interviews. Organizational Behavior and Human Decision Processes, 148, 1-11.
Recruiters care about intrinsic motivations of candidates (seeking a meaningful job, and having the "dream job" of a candidate match what is available), which candidates don't quite realize. So from a job search POV, the candidates should try to express a genuine interest in the kind of work you're applying for. From a hiring POV, it means that we are selecting for people who are good in expressing their interest, rather than for those who actually have an interest, or would be good at the job, and we're hiring suboptimally.

Aline Lerner. (2023) After a lot more data, technical interview performance really is kind of arbitrary.
https://interviewing.io/blog/after-a-lot-more-data-technical-interview-performance-really-is-kind-of-arbitrary
Their internal data, with nice visualizations (but somewhat confusing math). Results for the same person across different interviews is extremely variable, with standard deviation of about 30% of the average scare, meaning that among 5 interviews, most people would get both the max and the lowest score at least once. So it is really a numbers game.

# Cognitive tests

Kuncel, N. R., & Hezlett, S. A. (2010). Fact and fiction in cognitive ability testing for admissions and hiring decisions. _Current Directions in Psychological Science_, _19_(6), 339-345.
https://journals.sagepub.com/doi/pdf/10.1177/0963721410389459?casa_token=apoYkIPvH4IAAAAA:4U6Z4lcceg7zSlKESb0d2cAM5x9n9b8usV4d1yDXq1WsEehY4qwJElJTmNl7J-KAyLkUopnF8SG5
A review of literature, showing that job performance correlates with standardized tests of aptitude and personality traits. However for them r=0.6 (measrued cognitive ability vs high complexity job performance) seems high, and they are not ashamed to mention that high GPA is a "meaningful predictor" (with r=0.2). Very uncriticial of their own analysis. To exaggerate, the text is written as if they just randomly decided to understand "practically valid" as "statistically significant with very large samples", and then proceeded to write a paper about that. Which makes their claims technically true, but at the same time useless in practice. The analysis of biases is shallow, and with some "orange flags" in terms of how the accents are put.

Kuncel, N. R., Ones, D. S., & Sackett, P. R. (2010). Individual differences as predictors of work, educational, and broad life outcomes. _Personality and individual differences_, _49_(4), 331-336.
https://www.sciencedirect.com/science/article/pii/S0191886910001765?casa_token=IMaLMvi5nMEAAAAA:pAZ1p4J1gBYfSQW4m2FgrAgQ6D0EWZ5BSMnONSwHNBBvkLOy6oXxgIih9WYVPXWvieh21HyYUg
Almost identical to the previous one; same claims, but different language. For example, they find a ~0.4 correlation between SAT and college GPA; admit that it is decreased to r = 0.15-0.2 for racially uniform samples, and still call it "valid and reliable". Hashtag shrug, hashtag ok boomer.

Wee, S., Newman, D. A., & Joseph, D. L. (2014). More than g: Selection quality and adverse impact implications of considering second-stratum cognitive abilities. _Journal of applied psychology_, _99_(4), 547.
https://www.researchgate.net/profile/Serena-Wee/publication/287207336_Wee2014b/links/5673918e08aedbbb3f9fac7b/Wee2014b.pdf
When hiring based on tests, if you use total scores, you get rigid non-diverse teams with members following a certain "rounded" profile. The only way to get a more interesting team is to pick people with complementing skills by selecting winners of individual sub-scores. 
Start with a quick summary that cognitive tests work better than interviews and biographies (2 refs), but worse than work samples and assessments of past performance (4 refs).

# Personality tests

(as an alternative to [[behav_interview]])

The Validity and Utility of Selection Methods in Personnel Psychology: Practical and Theoretical Implications of 100 Years
https://home.ubalt.edu/tmitch/645/session%204/Schmidt%20&%20Oh%20validity%20and%20util%20100%20yrs%20of%20research%20Wk%20PPR%202016.pdf

# Opinion pieces

James Densmore, 2017. There are two types of data scientists — and two types of problems to solve
https://medium.com/@jamesdensmore/there-are-two-types-of-data-scientists-and-two-types-of-problems-to-solve-a149a0148e64
Type A (for "Analysis") like working with data, answering questions, applying methods, finding patterns, making sense of numbers, designing experiments, applying statistics. Type B (for "Builders") like developing new tools for production (more of a software engineer type), such as prediction systems, pipelines, etc. Two types of problems, two types of people, and they need to match, or everyone are unhappy.

 Ines Panker (2021) The Interview Process Isn't Broken, It Is Intolerant By Design.
 http://www.ines-panker.com/2021/06/10/the-interview-process-isnt-broken-it-is-intolerant-by-design.html
*  Most people don't have a clear rubric: don't bother to define and justify why each question was chosen; how good and bad answers to this quesetion look like, and why. 
*  In practice it means that the search is noisy, and also biased to questions that we randomly know (or think of), and answers that we ourselves would have given
*  Most people interview without a good training (and most trainings given to people are bad)