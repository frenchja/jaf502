---
layout: post
title: "Using Figures within Tables in LaTeX"
date: 2012-01-17 18:23
comments: true
categories: LaTeX
---
By using LaTeX to author APA manuscripts, researchers can address many problems associated with formatting their results
into tables and figures. For example, ANOVA tables can be readily generated using the
[`xtable`](http://cran.r-project.org/web/packages/xtable/index.html) package in R, and graphs from
[`ggplot2`](http://cran.r-project.org/web/packages/ggplot2/index.html) can be rendered *within* the manuscript using
`Sweave` (see [Wikipedia](https://en.wikipedia.org/wiki/Sweave)). However, more complicated layouts can be difficult to
achieve.

In order to make test items or stimuli easier to understand, researchers occasionally organize examples in a table or
figure. Using the standard `\table` command in LaTeX, it's possible to include figures in an individual table cell
without breaking the [APA6.cls](http://ctan.org/pkg/apa6) package. For example: <!-- more -->

```latex Bottom-Aligned Figures in LaTeX Tables
\begin{table}
	[ht] \caption{Stimuli Category Explanations} \label{tab:stimuli} 
	\begin{tabular}
		{lllll} \hline Category & Familiar Organism & Growth Model & Metamorphosis & Figure \\
		\hline \textbf{FDM} & Yes & Dramatic & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-Dramatic} \\
		FDN & Yes & Dramatic & No &
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Dramatic} \\
		FGM & Yes & Growth & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-Growth} \\
		\textbf{FGN} & Yes & Growth & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Growth} \\
		FIM & Yes & Identical & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-ID} \\
		FIN & Yes & Identical & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Identical} \\
		FSM & Yes & Species Change & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-Species} \\
		FSN & Yes & Species Change & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Species} \\
		\textbf{UDM} & No & Dramatic & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Dramatic} \\
		UDN & No & Dramatic & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Dramatic} \\
		UGM & No & Growth & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Growth} \\
		\textbf{UGN} & No & Growth & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Growth} \\
		UIM & No & Identical & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-ID} \\
		UIN & No & Identical & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Identical} \\
		USM & No & Species Change & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Species} \\
		USN & No & Species Change & No & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Species} \\
		USM & No & Species Change & Yes & 
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Species} \\
		\hline 
	\end{tabular}
	\tabfnt{Note: Bold categories are biologically correct growth models.} 
\end{table}
```

## Center-Aligned Figures
However, the above code vertically aligned my images according to their bottom-edge, producing an awkward looking table.
Instead, we want the figures to be vertically centered. A Google search revealed the [LaTeX
Wikibook](https://en.wikibooks.org/wiki/LaTeX/Tables#Vertically_centered_images), which suggests a few methods to force
figures to vertically align according to their center. Below, I surround each `\includegraphics{}` command with the
`\parbox{}` command, which centers it along 1 unit of measurement, set to 12 pts. in my apa6 class options.


{% codeblock Center-Aligned Figures in LaTeX Tables lang:latex %}
\begin{table}
	[ht] \caption{Stimuli Category Explanations} \label{tab:stimuli} 
	\begin{tabular}
		{lllll} \hline Stimuli Category & Familiar Organism & Growth Model & Metamorphosis & Figure \\
		\hline \textbf{FDM} & Yes & Dramatic & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-Dramatic}} \\
		FDN & Yes & Dramatic & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Dramatic}} \\
		FGM & Yes & Growth & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-Growth}} \\
		\textbf{FGN} & Yes & Growth & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Growth}} \\
		FIM & Yes & Identical & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-ID}} \\
		FIN & Yes & Identical & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Identical}} \\
		FSM & Yes & Species Change & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Meta-Species}} \\
		FSN & Yes & Species Change & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Fam-Species}} \\
		\textbf{UDM} & No & Dramatic & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Dramatic}} \\
		UDN & No & Dramatic & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Dramatic}} \\
		UGM & No & Growth & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Growth}} \\
		\textbf{UGN} & No & Growth & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Growth}} \\
		UIM & No & Identical & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-ID}} \\
		UIN & No & Identical & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Identical}} \\
		USM & No & Species Change & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Species}} \\
		USN & No & Species Change & No & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un-Species}} \\
		USM & No & Species Change & Yes & \parbox[c]{1em}{
		\includegraphics[width=1in]{"Figure Stimuli"/Un2-Meta-Species}} \\
		\hline 
	\end{tabular}
	\tabfnt{Note: Bold categories are biologically correct growth models.} 
\end{table}
{% endcodeblock %}
Output:
{% img center /images/origins.png 'Broken LaTeX Table' %}

By using `\parbox`, figures are now vertically aligned with text cells. However, with the addition of figures the table
is too long and we must span the table across 2-pages. To solve this, split the information across two tables. In this
case, I can split by the **stimuli category**.

Alternatively, [Brian Beitzel](http://www.edpsych.net/brian/) also pointed out that we can invoke
[`longtable`](http://tug.ctan.org/pkg/longtable) as a class option in [APA6.cls](http://ctan.org/pkg/apa6), which allows
tables to span multiple pages.