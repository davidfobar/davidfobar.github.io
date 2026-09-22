# Decision log

Your methods section. About one page total.

Answer these as you go, not the night before it is due.
Specifics beat polish - a short honest answer is worth more than a long vague one.

Delete these instructions when you are done, or leave them. It does not matter.

---

## 1. What did you set out to build, and what changed?

What you wanted at the start, and what is actually live now.
Name one thing you dropped or added along the way, and why.

I wanted to build a website that I can use to document my personal projects - mostly for myself, but also as an extension to a resume. I also wanted to have a place to easily link my publications. As teh project progressed, I realized that I could make some of the content active, for example the automated foucault test post includes an applet that I find very useful for understanding the mirror tester.

---

## 2. A fork in the road

Name one real choice where you could have gone two ways.
Plain HTML or a framework. One page or several. Your own CSS or someone's template.
What goes on the front page and what does not.

Say which you picked, what the alternative was, and what you gave up by not taking it.

"There was no alternative" is not an answer. Find the fork.

I debated using jekyll or not, I ultimately decided that I wanted to focus on content rather than creation. 

---

## 3. Where you overruled the agent

One time Claude suggested, wrote, or claimed something and you did not take it.

What did it do? How did you notice? What did you do instead?

If it genuinely never happened, say so plainly, and then say what you would have had to
check in order to notice. Being honest here costs you far less than a story you cannot
defend when you record your video.

I asked Claude to generate a post about Squatober, and I did not provide enough context, and it clearly did not have the context within its training, so the post was completely off the target. Rather than fight it, I choose to omit the post altogether, maybe I will put something together later. But in the spirit of the project, all of the current posts are 100% generated.

---

## 4. How you know it works

What check did you run, and what did it tell you?

Then the real question: **what would have made this check fail?**
A check that could not have failed is not a check.

Link to your `verification/` folder.

I built the site locally, and incrementially pushed to github. I also maintained a local conversation summary file that I had claude generate. The summary paired with the commit messages helped troubleshoot some issues.

---

## 5. What is still wrong

One thing on your own site that is not right, not finished, or that you do not
fully understand.

What would you do next, and how would you find out?

I wanted to use the URL platypusworx.net, but I still need to figure out how to get the content at that URL. I also have a few more posts in mind with interactive content to help describe the issue to be solved in the project. I also have many more projects to add.
