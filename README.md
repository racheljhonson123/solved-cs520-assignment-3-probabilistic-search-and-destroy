Download Link: https://assignmentchef.com/product/solved-cs520-assignment-3-probabilistic-search-and-destroy
<br>
The purpose of this assignment is to model your knowledge/belief about a system probabilistically, and use this belief state to efficiently direct future action.

<strong>The Problem: </strong>Consider a landscape represented by a map of cells. The cells can be of various terrain types (‘<strong>flat</strong>’, ‘<strong>hilly</strong>’, ‘<strong>forested</strong>’, ‘<strong>a complex maze of caves and tunnels</strong>’) representing how difficult they are to search. Hidden somewhere in this landscape is a <strong>Target</strong>. Initially, the target is equally likely to be anywhere in the landscape, hence starting out:

P(Target in Cell<em>.                                   </em>(1)

# of cells

This represents your <strong>prior belief </strong>about where the target is. You have the ability to <strong>search </strong>a given cell, to determine whether or not the target is there. However, the more difficult the terrain, the harder is can be to search – the more likely it is that even if the target is there, you may not find it (<strong>a false negative</strong>). We may summarize this in terms of the false negative rates:

1        if Cell<em><sub>i </sub></em>is <strong>flat</strong>

<table width="584">

 <tbody>

  <tr>

   <td width="333">0<em>.</em>3P(Target not found in Cell<em><sub>i</sub></em>|Target is in Cell<em><sub>i</sub></em>) =</td>

   <td width="233">if Cell<em><sub>i </sub></em>is <strong>hilly </strong>if Cell<em><sub>i </sub></em>is <strong>forested</strong>if Cell<em><sub>i </sub></em>is <strong>a maze of caves</strong></td>

   <td width="17">(2)</td>

  </tr>

 </tbody>

</table>

The false positive rate for any cell is taken to be 0, i.e., P(Target found in Cell<em><sub>i</sub></em>|Target not in Cell<em><sub>i</sub></em>) = 0. Whatever the result of a search, however, it does provide <strong>some </strong>useful information about the location of the target, lowering the likelihood of the target being in the searched cell and raising the likelihood of it being in others.

If you find the target, you are done. The goal is to locate the target in <strong>as few searches as possible </strong>by utilizing the results of the observations collected.

<strong>Implementation: </strong>Maps may be generated in the following way: for a 50 by 50 grid, randomly assign each cell a terrain type, (flat with probability 0<em>.</em>2, hilly with probability 0<em>.</em>3, forested with probability 0<em>.</em>3, and caves with probability 0<em>.</em>2). Select a cell uniformly at random, and identify this as the location of the target. Note, this information is going to be stored and available in your program, but you <strong>cannot </strong>use it to determine which cell to check at any time.

Figure 1: A simple 10 x 10 map with indicated target.

Separately, construct a 50 x 50 array of values representing your current <strong>belief state</strong>, the probability <em>given everything you’ve observed so far </em>that the target is in a given cell, i.e., at time <em>t</em>

Belief[Cell<em><sub>i</sub></em>] = P(Target in Cell<em><sub>i</sub></em>| Observations through time <em>t</em>)<em>.         </em>(3)

Note, at time <em>t </em>= 0, we have Belief[Cell<em><sub>i</sub></em>] = 1<em>/</em>2500.

At any time <em>t</em>, given the current state of <strong>Belief</strong>, determine a cell to check by some rule and <strong>search it</strong>. If the cell does not contain the target, the search will return <strong>failure</strong>. If the search does contain the target, the search will return <strong>failure </strong>or <strong>success </strong>with the appropriate probabilities, based on the terrain type. If the search returns failure, for whatever reason, use this observation about the selected cell to update your belief state for <em>all </em>cells (using Bayes).

If the search returns success, return the total number of searches taken to locate the target.

<h1>1         A Stationary Target</h1>

<ul>

 <li>Given observations up to time <em>t </em>(Observations<em><sub>t</sub></em>), and a failure searching Cell <em>j </em>(Observations<em><sub>t</sub></em><sub>+1 </sub>= Observations<em><sub>t</sub></em>∧ Failure in Cell<em><sub>j</sub></em>), how can Bayes’ theorem be used to efficiently update the belief state, i.e., compute:</li>

</ul>

P(Target in Cell<em><sub>i</sub></em>|Observations<em><sub>t </sub></em>∧ Failure in Cell<em><sub>j</sub></em>)<em>.              </em>(4)

<ul>

 <li>Given the observations up to time <em>t</em>, the belief state captures the <strong>current probability the target is in a given cell</strong>. What is the probability that the target will be <strong>found </strong>in Cell <em>i </em>if it is searched:</li>

</ul>

P(Target found in Cell<em><sub>i</sub></em>|Observations<em><sub>t</sub></em>)?                       (5)

<ul>

 <li>Consider comparing the following two decision rules:

  <ul>

   <li>Rule 1: At any time, search the cell with the highest probability of containing the target.</li>

   <li>Rule 2: At any time, search the cell with the highest probability of finding the target.</li>

  </ul></li>

</ul>

For either rule, in the case of ties between cells, consider breaking ties arbitrarily. How can these rules be interpreted / implemented in terms of the known probabilities and belief states?

For a fixed map, consider repeatedly using each rule to locate the target (replacing the target at a new, uniformly chosen location each time it is discovered). On average, which performs better (i.e., requires less searches), Rule 1 or Rule 2? Why do you think that is? Does that hold across multiple maps?

<ul>

 <li>Consider modifying the problem in the following way: at any time, you may only search the cell at your current location, or move to a neighboring cell (up/down, left/right). Search or motion each constitute a single ‘action’. In this case, the ‘best’ cell to search by the previous rules may be out of reach, and require travel. One possibility is to simply move to the cell indicated by the previous rules and search it, but this may incur a large cost in terms of required travel. How can you use the belief state and your current location to determine whether to search or move (and <em>where </em>to move), and minimize the total number of actions required? Derive a decision rule based on the current belief state and current location, and compare its performance to the rule of simply always traveling to the next cell indicated by <strong>Rule 1 </strong>or <strong>Rule 2</strong>. Discuss.</li>

 <li>An old joke goes something like the following:</li>

</ul>

<em>A policeman sees a drunk man searching for something under a streetlight and asks what the drunk has lost.</em>

<em>He says he lost his keys and they both look under the streetlight together. After a few minutes the policeman asks if he is sure he lost them here, and the drunk replies, no, and that he lost them in the park. The policeman asks why he is searching here, and the drunk replies, ”the light is better here”.</em>

In light of the results of this project, discuss.

<h1>2         A Moving Target</h1>

In this section, the target is no longer stationary, and can move between neighboring cells. Each time you perform a search, if you fail to find the target the target will move to a neighboring cell (with uniform probability for each). However, all is not lost – whenever the target moves, surveillance reports to you that the target was seen at a <strong>Type1 x Type2 </strong>border where Type1 and Type2 are the cell types the target is moving between (though it is not reported which one was the exit point and which one the entry point.

Implement this functionality in your code. How can you update your search to make use of this extra information? How does your belief state change with these additional observations? Update your search accordingly, and again compare <strong>Rule 1 </strong>and <strong>Rule 2</strong>.

Re-do question 4) above in this new environment with the moving target and extra information.