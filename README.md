# SingleTransferableVote
Code for finding upward, downward, and no-show anomalies in multiwinner STV elections

Each set of code takes in a preference schedule (usually a txt or csv file) of the following form:

5 3

37 2 4 5 0

62 3 0

3 3 1 4 2 5 0

0

The first line is the number of candidates (in this case, 5) followed by the number of seats to be filled (in this case, 3). Inserting a 1 for the number of seats effectively runs a single-winner instant runoff election.
The remaining lines are of the form (# of ballots) (ranking) 0, so (for example) there are 37 ballots with the ranking of candidate 2 in first place, 4 in second, and 5 in third place.  Each type of ballot ends with a 0 to tell the code where the line ends.  After all ballots are listed, there is a single line with a 0. Any data included in the file after that 0 is not read.

Each set of code first runs the STV election to see who the winner set is, what the order of elimination of the losing candidates is, and the preference schedule at each point where a candidate is eliminated.

The upward anomaly code searches for every point where a candidate is eliminated.  At each such point, the code looks for ballot changes where moving a winning candidate up in some ballots would cause a change in the elimination order of the candidates.  If such a change is possible, it makes the change and then runs an STV election (from that point) on the modified ballots.  If the original winner who was moved up is no longer a winner, it reports an anomaly.

The downward anomaly code has two parts: the first searches for every point where a candidate is eliminated.  At each such point, the code looks for ballot changes where moving a losing candidate down in some ballots would cause a change in the elimination order of the candidates.  If such a change is possible, it makes the change and then runs an STV election (from that point) on the modified ballots.  If the original loser who was moved down is now a winner, it reports an anomaly.  The second part does the same thing, but looks instead at each point where a candidate is elected.  So far, we have not found any anomalies connected to the "Seat Order" modifications.

The no show anomaly code searches for every point where a candidate is eliminated.  At each such point, the code looks for ballot changes where eliminating ballots of a certain form (see ** for details) to benefit a losing candidate C and hurt a winner W would cause a change in the elimination order of the candidates.  If eliminating such ballots is possible, it eliminates those ballots and then runs an STV election (from that point) on the modified election.  If C is now a winner and W is now a loser AND no other winning candidates were affected, it reports an anomaly.

**The "certain form" are ballots L...C...W, that is, ballots where a different losing candidate (L) has C ranked above W.  When those ballots are removed, candidate L receives a better result than they would otherwise have recevied, since C replaces W in the winner set.
