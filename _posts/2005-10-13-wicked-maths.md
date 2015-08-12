---
title: Wicked Maths
author: Ivan Zlatev
layout: post
permalink: /2005/10/13/wicked-maths/
views:
  - 18
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1321088
categories:
  - Coding
---
Yesterday I had a lecture in Quantitive Methods for computing, which was very wicked. We had a text, which the lecturer proved with logic maths. Here it is:

`<br />
1) If the software is robust, -----> R<br />
or the project is under budget, -----> B<br />
then the team leader is happy. -----> H<br />
2)If the team leader is happy,<br />
then the team get a bonus. -----> P<br />
3)The team don't get a bonus.<br />
------------------------------------------<br />
Therefore the software is not robust<br />
`

Solution:  
`<br />
1)  (R or B)  => H<br />
2)  H => P<br />
3)  not(P)<br />
-------------------------<br />
      not(R)<br />
`

Here is the scheme:

`<br />
(R or B) => H<br />
H => P<br />
------------------<br />
(R or B) => P   - Hypothetical Syllogism</p>
<p>(R or B) => P<br />
not (P)<br />
------------------<br />
not(R or  B)    -  Modus Tollens</p>
<p>not(R or B)  -> not(R) and not(B)   - De Morgan</p>
<p>not(R) and not(B) -> not(R)   - Simplicfication<br />
`