---
layout: post
title: Stable Matching Problem
categories: 
  - Algorithm
tags: 
  - Stable matching
published: true
---

##Problems and Solutions
**2.** State True or false? In every instance of the Stable Matching Problem, there is a stable matching containing a pair (m, w) such that m is ranked first on the preference list of w and w is ranked first on the preference list of m.

**False.** Consider this situation for two men and two women instance:

* m prefers w to w'
* m' prefers w' to w
* w prefers m' to m'
* w' prefers m to m 

where w ranks first in preference list for m, while m is not first on preference list for w. Clearly, this stable matching contains a pair (m,w) without such a property.

**3.** State True or false? Consider an instance of the Stable Matching Problem in which there exists a man m and a woman w such that m is ranked first on the preference list of w and w is ranked first on the preference list of m. Then in every stable matching S for this instance, the pair (m, w) belongs to S.

**True.** Consider this situation:

* m prefers w' to w
* m' prefers w to w'
* w prefers m to m'
* w' prefers m' to m 

 
This is a perfect matching situation where two men are both happy and both women are unhappy. S includes (m,w') and (m',w). However, m and w both rank each other on their top of preference list. Nothing can stop (m, w) bing a pair, which indicates that situation is unstable. 



**4.** State True/False: An instance of the stable marriage problem has a unique stable matching if and only if the version of the Gale-Shapely algorithm where the male proposes and the version where the female proposes both yield the exact same matching.

**True.** If the male proposes and the version where the female proposes both yield the exact same matching, then male female have the same preference list, leading a unique stable matching. 


**5.** A stable roommate problem with 4 students a, b, c, d is defined as follows. Each student ranks the other three in strict order of preference. A matching is defined as the separation of the students into two disjoint pairs. A matching is stable if no two separated students prefer each other to their current roommates. Does a stable matching always exist? If yes, give a proof. Otherwise give an example roommate preference where no stable matching exists.

**No. A stable matching may not exist.** For instance, the preference list for a, b, c, d could be:

* a: c, b, d
* b: d, c, a
* c: a, d, b
* d: b, a, c

However, the current matching is (a, b), and (c, d). This is unstable because nothing can stop them to swap roommates to matching as (a, c), (b, d), which is stable.


**6.** There are many other settings in which we can ask questions related to some type of “stability” principle. Here’s one, involving competition between two enterprises.Suppose we have two television networks, whom we’ll call A and B. There are n prime-time programming slots, and each network has n TV shows. Each network wants to devise a schedule—an assignment of each show to a distinct slot—so as to attract as much market share as possible.

Here is the way we determine how well the two networks perform relative to each other, given their schedules. Each show has a fixed rating, which is based on the number of people who watched it last year; we’ll assume that no two shows have exactly the same rating. A network wins a given time slot if the show that it schedules for the time slot has a larger rating than the show the other network schedules for that time slot. The goal of each network is to win as many time slots as possible.

Suppose in the opening week of the fall season, Network A reveals a schedule S and Network B reveals a schedule T. On the basis of this pair of schedules, each network wins certain time slots, according to the rule above. We’ll say that the pair of schedules (S, T) is stable if neither network can unilaterally change its own schedule and win more time slots. That is, there is no schedule S′ such that Network A wins more slots with the pair (S′, T) than it did with the pair (S, T); and symmetrically, there is no schedule T′ such that Network B wins more slots with the pair (S, T′) than it did with the pair (S, T).

The analogue of Gale and Shapley’s question for this kind of stability is the following: For every set of TV shows and ratings, is there always a stable pair of schedules? Resolve this question by doing one of the following two things:
 
- give an algorithm that, for any set of TV shows and associated ratings, produces a stable pair of schedules; or
	
- give an example of a set of TV shows and associated ratings for which there is no stable pair of schedules.

**No.** Assume A has two shows a1, a2 with ratings 1 and 3, and B has two shows b1, b2 with ratings 2 and 4. If there is a matching: S: (a1, a2), T:(b1, b2) with paris (a1, b1) and (a2, b2). Then b1 and b2 will win two time slots and network A must want to change their schedule as S':(a2, a1) which can win back one time slot and thus it becomes stable.

**7.** Gale and Shapley published their paper on the Stable Matching Problem in 1962; but a version of their algorithm had already been in use for ten years by the National Resident Matching Program, for the problem of assigning medical residents to hospitals.

Basically, the situation was the following. There were m hospitals, each with a certain number of available positions for hiring residents. There were n medical students graduating in a given year, each interested in joining one of the hospitals. Each hospital had a ranking of the students in order of preference, and each student had a ranking of the hospitals in order of preference. We will assume that there were more students graduating than there were slots available in the m hospitals.

The interest, naturally, was in finding a way of assigning each student to at most one hospital, in such a way that all available positions in all hospitals were filled. (Since we are assuming a surplus of students, there would be some students who do not get assigned to any hospital.)


We say that an assignment of students to hospitals is stable if neither of the following situations arises.

- First type of instability: There are students s and s′, and a hospital h, so that
	- s is assigned to h, and
	- s′ is assigned to no hospital, and 
	- h prefers s′ to s.
- Second type of instability: There are students s and s′, and hospitals h and h′, so that
	- s is assigned to h, and
	- s′ is assigned to h′, and
	- h prefers s′ to s, and
	- s′ prefers h to h′.

So we basically have the Stable Matching Problem, except that (i) hospitals generally want more than one resident, and (ii) there is a surplus of medical students.

Show that there is always a stable assignment of students to hospitals, and give an algorithm to find one.

```
While some hospital h_{i} has available positions
	h_{i} offers a position to the next student s_{j} on its preference list
	if s_{j} is free then
		s_{j} accepts the offer
	else (s_{j} is already committed to a hospital h_{k})
		if s_{j} prefers h_{k} to h_{i} then
			s_{j} remains committed to h_{k}
		else s_{j} becomes committed to h_{i}
			the number of available positions at h_{k} increases by 1
			the number of available positions at h_{i} decreases by 1
```	

**8.** N men and N women were participating in a stable matching process in a small town named Walnut Grove. A stable matching was found after the matching process finished and everyone got engaged. However, a man named Almazo Wilder, who is engaged with a woman named Nelly Oleson, suddenly changes his mind by preferring another woman named Laura Ingles, who was originally ranked right below Nelly in his preference list, therefore Laura and Nelly swapped their positions in Almanzos preference list. Your job now is to find a new matching for all of these people and to take into account the new preference of Almanzo, but you dont want to run the whole process from the beginning again, and want to take advantage of the results you currently have from the previous matching. Describe your algorithm for this problem. Assume that no woman gets offended if she got refused and then gets proposed by the same person again.

**Solution:** Swap Nelly Oleson and Laura Ingles.