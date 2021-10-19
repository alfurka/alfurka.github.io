---
layout: post
title: Custom Theorem, Assumption, and Lemma Numbering in Latex
subtitle: Examples of Customized Theorem Names
tags: [latex, newtheorem, theorem, assumption,lemma,numbering]
image: /img/hello_world.jpeg
bigimg: /img/code.jpg
---

I saw this kind of alternative assumption definition in other papers, such as:

- Assumption 1: ...
- Assumption 2: ...
- Assumption 2': ...

Here we define two alternative assumptions. How to do it in Latex with `\newtheorem' command. For example, if we define an assumption in the following way:

```latex
\newtheorem{assumption}{Assumption}

....

\begin{assumption}
 First one
\end{assumption}

\begin{assumption}
 Second one
\end{assumption}
```

![Latex Assumptions](/img/assumption-latex-1.JPG)

I now define the following `assumptionp`:

```latex
\newtheorem{assumption}{Assumption}

% Alternative Assumption!
\newtheorem{assumptionalt}{Assumption}[assumption]

% New environment
\newenvironment{assumptionp}[1]{
  \renewcommand\theassumptionalt{#1}
  \assumptionalt
}{\endassumptionalt}
```

For alternative assumption, we define `assumptionalt` as a new theorem. It uses `assumption` as parent counter ([assumption]). Then we define a new environment `assumptionp`. It takes one argument, which will allow us to customize the section name. Examples:

```latex
\begin{assumption}
First one
\end{assumption}

\begin{assumption}\label{second}
Second one
\end{assumption}

\begin{assumptionp}{\ref{second}$'$}
Second one alternative
\end{assumptionp}

\begin{assumptionp}{\ref*{second}$'$}
Second one alternative with no hyper link
\end{assumptionp}

\begin{assumptionp}{C}
Custom One
\end{assumptionp}
```

It looks like this:

![Custome Assumption Alternatives Latex](/img/assumption-latex-2.JPG)

I derived all these from StackOverflow. It took me a while to understand it. You can easily replace assumption with theorem, lemma, etc. 
