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

False. Suppose we have two men m, m0 and two women w, w0. Let m rank w first; w rank m0 first; m0 rank w0 first; and w0 rank m first. We see that such a pair as described by the claim does not exist.

**3.** State True or false? Consider an instance of the Stable Matching Problem in which there exists a man m and a woman w such that m is ranked first on the preference list of w and w is ranked first on the preference list of m. Then in every stable matching S for this instance, the pair (m, w) belongs to S.

True. Consider an instance with a man m and a woman w such that m is ranked first on the preference list of w and w is ranked first on the preference list of m. Suppose (for contradiction) that S is a stable matching that contains the pairs (m, w0) and (m0, w), for some m0 ̸= m, w0 ̸= w. Clearly, the pairing (m,w) is preferred by both m and w over their current pairings, contradicting the stability of S.

**4.** State True/False: An instance of the stable marriage problem has a unique stable matching if and only if the version of the Gale-Shapely algorithm where the male proposes and the version where the female proposes both yield the exact same matching.

True.Claim : ”If there is a unique stable matching then the version of the Gale-Shapely algorithm where the male proposes and the version where the female proposes both yield the exact same matching.”The proof of the above claim is clear by virtue of the correctness of Gale- Shapely algorithm. That is, the version of the Gale-Shapely algorithm where the male proposes and the version where the female proposes are both correct and hence will both yield a stable matching. However since there is a unique stable matching, both versions of the algorithms should yield the same matching.
Converse of Claim: ”If the version of the Gale-Shapely algorithm where the male proposes and the version where the female proposes both yield the exact same matching then there is a unique stable matching.”The proof of the converse is perhaps more interesting. For the definition of best valid partner, worst valid partner etc., see page 10-11 in the textbook.
Let S denote the stable matching obtained from the version where men propose and let S0 be the stable matching obtained from the version where women propose.From (page 11, statement 1.8 in the text), in S, every woman is paired with her worst valid partner. Applying (page 10, statement 1.7) by symmetry to the version of Gale-Shapely where women propose, it follows that in S0, every woman is paired with her best valid partner. Since S and S0 are the same matching, it follows that for every woman, the best valid partner and the worst valid partner are the same. This implies that every woman has a unique valid partner which implies that there is a unique stable matching.

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

**Solution:** 
First, Almanzo leaves Nelly and proposes to Laura:

- If Laura rejects Almanzo and decides to stay with her current fianc (her current fianc is ranked higher than Almanzo in her preference list), then Almanzo just goes back and proposes to Nelly again and gets engaged with her. Algorithm stops. This is where you take advantage of the previous matching.
- Else, Laura accepts Almanzo proposal, she will get engaged with him and leave her current fiance (say Adam). And here comes the main part of the matching puzzle.
 - Note that Adam may or may not simply propose to Nelly and be engaged with her. The reason is Nelly may be far below Laura in his preference list and there are more women (worse than Laura and better than Nelly), to whom he prefers to propose first. Furthermore, the fact that Nelly becomes single can create more instabilities for those men, who once proposed to Nelly and were rejected by her because she preferred Almanzo. Thus, Nelly is ranked higher than the current fiances of those once unlucky men and they are ready to propose to Nelly again for a second chance.
 - Thus, one way is to put Adam and all those rejected-by-Nelly men in the pool of single men, and put Nelly and the fiancees of the rejected-by-Nelly men in the pool of free women. Similar to Nellys case, moving any woman from the engaged state to the single state may create more instabilities; thus one needs execute step (b) recursively to build the pools of free men and women. In the worst case, all men and all women will end up in the single pools; in the best case (when Laura accepted Almanzos proposal), only Adam and Nelly are in the 2 single pools, one for men and another for women. 
 - Execute G-S algorithm until there is no free man. For each man, if there is a woman who once rejected him and is now free, he will try to propose to her again. Otherwise, he will propose to those women that he has never proposed to, moving top down in his preference list. In the latter case, more men (subsequently more women) may go to the single pools.Note: The above solution is described in details in order to clarify the problem. For this problem, students can simply write pseudo code without having to discuss the details of each case as above.