@def prepath = "Zurich_Log"
@def author = "Langyan Zang"
@def title = "苏黎世学习日志"
@def hasmath = true              <!-- mostly there's maths on pages -->
@def hascode = true
@def generate_rss = false

@def mintoclevel = 2              <!-- TOCS only for level h2 and higher -->
@def maxtoclevel = 3              <!-- TOCS only up to level 3 included -->

@def hasplotly = false

<!--
Useful HTML snippets
* \blurb{...} for a blurb at the top of a page
* \refblank{...} for a link with target blank
* \lineskip forces skipping of a line somewhere
-->

\newcommand{\blurb}[1]{
    ~~~
    <span style="font-size:18px;font-weight:300;">!#1</span>
    ~~~
}
\newcommand{\refblank}[2]{
    ~~~
    <a href="!#2" target="_blank" rel="noopener noreferrer">#1</a>
    ~~~
}
\newcommand{\lineskip}{@@blank@@}
\newcommand{\skipline}{\lineskip}
\newcommand{\note}[1]{@@note @@title ⚠ Note@@ @@content #1 @@ @@}
\newcommand{\warn}[1]{@@warning @@title ⚠ Warning!@@ @@content #1 @@ @@}
\newcommand{\theorem}[2]{@@theorem @@title 🐣 Theorem: #1@@ @@content #2 @@ @@}
\newcommand{\proof}[2]{@@proof @@title 🤯 Proof: #1@@ @@content #2 @@ @@}
\newcommand{\intuition}[1]{@@intuition @@title ䷀ Intuition@@ @@content #1 @@ @@}
\newcommand{\algo}[2]{@@algorithm @@title 🐉 #1 Algorithm:@@ @@content #2 @@ @@}

\newcommand{\esc}[2]{
    ```julia:esc__!#1
    #hideall
    using Markdown
    println("\`\`\`\`\`plaintext $(Markdown.htmlesc(raw"""#2""")) \`\`\`\`\`")
    ```
    \textoutput{esc__!#1}
}

\newcommand{\esch}[2]{
    ```julia:esc__!#1
    #hideall
    using Markdown
    println("\`\`\`\`\`html $(Markdown.htmlesc(raw"""#2""")) \`\`\`\`\`")
    ```
    \textoutput{esc__!#1}
}

\newcommand{\span}[2]{~~~<span style="display:inline-block;!#1">~~~!#2~~~</span>~~~}

\newcommand{\goto}[1]{~~~<a href="!#1" id="goto"><span id="check">&check;</span><span id="arrow"><b>&rarr;</b></span></a>~~~}

\newcommand{\smindent}[1]{\span{width:45px;text-align:right;color:slategray;}{#1}}
\newcommand{\smnote}[1]{\style{font-size:85%;line-height:0em;}{#1}}
