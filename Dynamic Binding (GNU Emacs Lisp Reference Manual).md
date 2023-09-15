<!DOCTYPE html>
<html><!-- Created by GNU Texinfo 7.0.3, https://www.gnu.org/software/texinfo/ --><head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
<title>Dynamic Binding (GNU Emacs Lisp Reference Manual)</title>

<meta name="description" content="Dynamic Binding (GNU Emacs Lisp Reference Manual)">
<meta name="keywords" content="Dynamic Binding (GNU Emacs Lisp Reference Manual)">
<meta name="resource-type" content="document">
<meta name="distribution" content="global">
<meta name="Generator" content="makeinfo">
<meta name="viewport" content="width=device-width,initial-scale=1">

<link rev="made" href="mailto:bug-gnu-emacs@gnu.org">
<link rel="icon" type="image/png" href="https://www.gnu.org/graphics/gnu-head-mini.png">
<meta name="ICBM" content="42.256233,-71.006581">
<meta name="DC.title" content="gnu.org">
<style type="text/css">
@import url('/software/emacs/manual.css');
</style>
</head>

<body lang="en">
<div class="subsection-level-extent" id="Dynamic-Binding">
<div class="nav-panel">
<p>
Next: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Dynamic-Binding-Tips.html" accesskey="n" rel="next">Proper Use of Dynamic Binding</a>, Up: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Variable-Scoping.html" accesskey="u" rel="up">Scoping Rules for Variable Bindings</a> &nbsp; [<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/index.html#SEC_Contents" title="Table of contents" rel="contents">Contents</a>][<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Index.html" title="Index" rel="index">Index</a>]</p>
</div>
<hr>
<h4 class="subsection" id="Dynamic-Binding-1">12.10.1 Dynamic Binding</h4>

<p>By default, the local variable bindings made by Emacs are dynamic
bindings.  When a variable is dynamically bound, its current binding
at any point in the execution of the Lisp program is simply the most
recently-created dynamic local binding for that symbol, or the global
binding if there is no such local binding.
</p>
<p>Dynamic bindings have dynamic scope and extent, as shown by the
following example:
</p>
<div class="example">
<div class="group"><pre class="example-preformatted">(defvar x -99)  ; <span class="r"><code class="code">x</code> receives an initial value of −99.</span>

(defun getx ()
  x)            ; <span class="r"><code class="code">x</code> is used free in this function.</span>

(let ((x 1))    ; <span class="r"><code class="code">x</code> is dynamically bound.</span>
  (getx))
     ⇒ 1

;; <span class="r">After the <code class="code">let</code> form finishes, <code class="code">x</code> reverts to its</span>
;; <span class="r">previous value, which is −99.</span>

(getx)
     ⇒ -99
</pre></div></div>

<p>The function <code class="code">getx</code> refers to <code class="code">x</code>.  This is a <em class="dfn">free</em>
reference, in the sense that there is no binding for <code class="code">x</code> within
that <code class="code">defun</code> construct itself.  When we call <code class="code">getx</code> from
within a <code class="code">let</code> form in which <code class="code">x</code> is (dynamically) bound, it
retrieves the local value (i.e., 1).  But when we call <code class="code">getx</code>
outside the <code class="code">let</code> form, it retrieves the global value (i.e.,
−99).
</p>
<p>Here is another example, which illustrates setting a dynamically
bound variable using <code class="code">setq</code>:
</p>
<div class="example">
<div class="group"><pre class="example-preformatted">(defvar x -99)      ; <span class="r"><code class="code">x</code> receives an initial value of −99.</span>

(defun addx ()
  (setq x (1+ x)))  ; <span class="r">Add 1 to <code class="code">x</code> and return its new value.</span>

(let ((x 1))
  (addx)
  (addx))
     ⇒ 3           ; <span class="r">The two <code class="code">addx</code> calls add to <code class="code">x</code> twice.</span>

;; <span class="r">After the <code class="code">let</code> form finishes, <code class="code">x</code> reverts to its</span>
;; <span class="r">previous value, which is −99.</span>

(addx)
     ⇒ -98
</pre></div></div>

<p>Dynamic binding is implemented in Emacs Lisp in a simple way.  Each
symbol has a value cell, which specifies its current dynamic value (or
absence of value).  See <a class="xref" href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Symbol-Components.html">Symbol Components</a>.  When a symbol is given
a dynamic local binding, Emacs records the contents of the value cell
(or absence thereof) in a stack, and stores the new local value in the
value cell.  When the binding construct finishes executing, Emacs pops
the old value off the stack, and puts it in the value cell.
</p>
<p>Note that when code using Dynamic Binding is native compiled the
native compiler will not perform any Lisp specific optimization.
</p>
</div>
<hr>
<div class="nav-panel">
<p>
Next: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Dynamic-Binding-Tips.html">Proper Use of Dynamic Binding</a>, Up: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Variable-Scoping.html">Scoping Rules for Variable Bindings</a> &nbsp; [<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/index.html#SEC_Contents" title="Table of contents" rel="contents">Contents</a>][<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Index.html" title="Index" rel="index">Index</a>]</p>
</div>





</body></html>