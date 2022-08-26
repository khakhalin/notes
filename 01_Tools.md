# Tools, coding, project management

#tools #lifehack #bib

Major subtopics:
* [[system_design]] - system design, scalability
* [[devops]] ?
* [[database]] - everything databases
* [[oop]] - on good software engineering, patterns, testing etc.

Tips and notes on individual tools:
* [[python]] - with separate notes for [[numpy]], [[pandas]] etc.
* [[tensorflow]], [[jax]] - ML frameworks
* [[sql]], [[postgres]] - Databases
* [[git]] - Version control
* [[bash]] - Scripting / terminal
* [[docker]], [[kubernetes]], [[flask]] - Containerization tools
* [[tableau]], [[kibana]], shiny (?) - Reporting tools
* [[javascript]], [[julia]] - other programming languages
* [[latex]] - misc

Running ML projects:
* [[ml_project_management]] - organizational principles
* [[ml_lore]] - a collection of rules of thumb about everything Deep Learning (which defaults to pick etc.)
* [[Breck2017testing]] - a rubric for ML project readiness
* [[data_cleaning]]

Project management and knowledge management:
* [[scrum]] - the methodology
* [[zettelkasten]] - how to properly create and support this knowledge base

# Good coding habits

* Keep code clean (not smelly). Types of **smells** ([ref](https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists)):
    1. Bad variable names (Remember: a long name is always better than a long comment!)
    2. Functions that do many things instead of one thing
    3. Code repetition
    4. Magic values (hard-coded values like 256, 0.9 etc.)
    5. Dead code (commented out, inconsequential)
    6. Print statements everywhere (ruins of abandoned debugging)
 
Instead:
* Write unit tests, and start with tests (aka test-driven development, see [[unit_test]])
* Make small and frequent commits
* Methods and functions:
    * should be small (~20 lines = 1 screen; better lesst than that), 
    * do one thing, 
    * at one level of abstraction (like, it shouldn't combine high-level processing of an object, and low-level processing of its parts; it should be dealt with by a hierarchy of functions, not raw code in one function)
    * have few arguments (ideally, 0-2)
    * avoid flags (`also_do_this=True`); consider writing 2 functions instead (_hmm? Is it true, and why? Not sure I necessarily understand how this is a help._)

# Misc tools

* The missing semester of CS education: https://missing.csail.mit.edu/
Lots of useful practical bits and pieces: the shell, debugging, metaprogramming and what not. Also has [lectures on youtube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J). Quite counter-intuitively, youtube lectures seem to be way more systematic than the notes. So maybe do the youtube.

Capture screen animation to a gif, client-side:
* online tool: https://gifcap.dev/	
* small downloadable tool: http://blog.bahraniapps.com/gifcam/