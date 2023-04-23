Download Link: https://assignmentchef.com/product/solved-algo-assignment2-hackerrank
<br>
The goal is to implement this with an algorithm whose running time is <em>O</em>(log(<em>n</em>)) using the “lazy update” strategy discussed in class (and in the 2-3 tree handout). You should use the following data layout:

class <em>Node </em>{

<em>KeyType </em>guide;

<em>ValueType </em>value;

}

class <em>InternalNode </em>extends <em>Node </em>{

<em>Node </em>child0, child1, child2;

} class <em>LeafNode </em>extends <em>Node </em>{}

Compared to Programming Assignment 1, this change essentially moves the value field from the <em>LeafNode </em>class to the base class <em>Node</em>. Recall that guide value at an internal node is used as a guide to facilitate search, while the guide value at a leaf represents the key stored at that leaf. The starter code you used for that programming assignment will still work with this modified data layout.

Recall that in this “lazy update” strategy, every node in the tree has a value field and the “effective value” associated with any key stored at a leaf is the sum of all the value fields in the nodes on the path from the root to that leaf.

In addition to implementing the <em>addRange </em>operation, you also have to implement the <em>lookup </em>operation and make a small modification to the <em>insert </em>operation in the starter code.

<h1>AddRange</h1>

Consider the following recursive algorithm which is initially invoked as

<em>addRange</em>(<em>p,x,y,h,lo,</em>∆)<em>,</em>

where <em>p </em>= root of tree, <em>h </em>= height of tree, and <em>lo </em>= −∞. It is assumed that all keys below <em>p </em>are <em>strictly </em>greater than <em>lo</em>.

<em>addRange</em>(<em>p,x,y,h,lo,</em>∆):

if <em>h </em>= 0 then

<em>// p points to a leaf node </em>if <em>p.</em>guide ∈ [<em>x,y</em>] then <em>p.</em>value ← <em>p.</em>value + ∆ return

<em>hi </em>← <em>p.</em>guide

<em>// p points to an internal node, and all keys below p lie in the interval </em>(<em>lo,hi</em>] if <em>y </em>≤ <em>lo </em>then return <em>// All leaves below p lie strictly to the right of </em>[<em>x,y</em>] if <em>hi &lt; x </em>then return <em>// All leaves below p lie strictly to the left of </em>[<em>x,y</em>] if <em>x </em>≤ <em>lo </em>and <em>hi </em>≤ <em>y </em>then

<em>// All leaves below p are in </em>(<em>x,y</em>] <em>p.</em>value ← <em>p.</em>value + ∆

return

<em>addRange</em>(<em>p.</em>child0<em>,x,y,h </em>− 1<em>,lo,</em>∆) <em>addRange</em>(<em>p.</em>child1<em>,x,y,h </em>− 1<em>,p.</em>child0<em>.</em>guide<em>,</em>∆)

if <em>p.</em>child2 6= null then <em>addRange</em>(<em>p.</em>child2<em>,x,y,h </em>− 1<em>,p.</em>child1<em>.</em>guide<em>,</em>∆)

This algorithm works much like the <em>printRange1 </em>algorithm from Programming Assignment 1. Like <em>printRange1</em>, this algorithm works by pruning the recursion as indicated when <em>y </em>≤ <em>lo </em>or <em>hi &lt; x</em>. However, it also prunes the recursion when <em>x </em>≤ <em>lo </em>and <em>hi </em>≤ <em>y</em>: when this condition occurs, we know that all leaves below <em>p </em>are in the range (<em>x,y</em>], and so we just add ∆ to <em>p.</em>value and recurse no further.

In general, this recursive algorithm will visit nodes on the search paths for <em>x </em>and <em>y</em>, which is a total of <em>O</em>(log(<em>n</em>)) nodes. The only additional nodes visited are where the pruning occurs, and these are siblings of nodes along the search paths for <em>x </em>and <em>y </em>— so there are only <em>O</em>(log(<em>n</em>)) such nodes.

To see in more detail why this works, recall the general statement, presented in the handout for Programming Assignment 1, about the relationship between the keys <em>lo </em>and <em>hi </em>associated with an internal node <em>p</em>, and the search paths for <em>x </em>and <em>y</em>. Consider the following diagram:

This represents a 2-3 tree showing search paths to <em>x </em>and <em>y</em>.

<ul>

 <li>Region <em>A </em>represents all internal nodes strictly to the left of the search path to <em>x</em>.</li>

</ul>

Node <em>p </em>is in this region ⇐⇒ <em>hi &lt; x</em>.

<ul>

 <li>Region <em>B </em>represents all internal nodes strictly between the two search paths.</li>

</ul>

Node <em>p </em>is in this region ⇐⇒ <em>x </em>≤ <em>lo </em>and <em>hi &lt; y</em>.

<ul>

 <li>Region <em>C </em>represents all internal nodes strictly to the right of the search path to <em>y</em>.</li>

</ul>

Node <em>p </em>is in this region ⇐⇒ <em>y </em>≤ <em>lo</em>.

If we wanted to implement directly the strategy outlined in lecture, we would prune the recursion by adding ∆ to <em>p.</em>value and stopping the recursion as soon as the recursion enters region <em>B</em>, i.e., when <em>x </em>≤ <em>lo </em>and <em>hi &lt; y</em>. However, in the above implementation of <em>addRange</em>, we are a bit more aggressive, and prune the recursion when <em>x </em>≤ <em>lo </em>and <em>hi </em>≤ <em>y</em>.

Finally, consider again the example from the handout for Programming Assignment 1:

Here, we prune the recursion at the internal nodes marked with a red X, as well as the internal nodes that are shaded gray — the algorithm adds ∆ to all the value fields of all of the gray nodes (both leaves and internal nodes). While this example illustrates the mechanics of this algorithm, it is too small to really illustrate how much faster it is than the more straightforward algorithm that would add ∆ to the value fields of all the leaves in the given range. However, to avoid time-out errors, it is essential that you use this strategy.

<h1>Lookup</h1>

You need to implement the lookup operation. Remember that to compute the “effective value” associated with a key, you have to add up all of the values on the search path from the root to the leaf containing that key.

<h1>Insertion</h1>

You also need to make a small adjustment to the insertion logic. The easiest way to modify the insertion logic is as follows. If you look at the starter code, you see that the insertion logic invokes a routine <em>doInsert </em>that recursively walks down the search path from the root to a leaf. You should modify this routine so that it has the effect of zeroing out the value fields of all internal nodes on this search path as it walks down this path. However, you have to do this in such a way that the “effective values” associated with the keys do not change. To do this, if an internal node <em>p </em>has a nonzero value <em>p.</em>value, you first add <em>p.</em>value to the value fields of each of <em>p</em>’s children, and then set <em>p.</em>value to 0. One can easily verify that this has the desired effect. (This strategy is analogous to “shoveling snow”.)

Once you zero out the value fields of all internal nodes on the search path, all of the other (rather tricky) logic of insertion (like splitting nodes) takes care of itself. Done right, all of this amounts to adding just a few lines of code to the <em>doInsert </em>routine. Here is an example:

In this example, we are inserting a new key between <em>k </em>and <em>l</em>. The search path would take us to the leaf containing the key <em>l</em>, as highlighted. In the top figure, we have indicated the value associated with each node along this path, as well as a few more for context. In the bottom figure, we have indicated the new values in red — except for the changes indicated, no other value fields in the tree will change. You can see that the root initially had a value of 2, but we clear out the value on the root (i.e., set the value field of the root to 0), and push it onto its children (i.e., we add 2 to the value fields of its children). Now when we come to the next node on the search path, while its value field was initially 1, it is now 3. So again, we clear out the value on this node, pushing it onto its children. We continue in this way all the way down the search path.

<h1>Summary</h1>

To summarize, for this programming assignment, you need to do the following:

<ul>

 <li>modify the data layout as discussed above;</li>

 <li>implement the <em>addRange </em>function as discussed above;</li>

 <li>implement the <em>lookup </em>function as discussed above;</li>

 <li>modify the <em>doInsert </em>function as discussed above.</li>

</ul>