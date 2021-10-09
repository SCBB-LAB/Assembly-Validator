# Assembly-Validator
 A tool to detect misassembly in de novo assembled contigs
De novo clustering of contigs into two groups as correctly assembled or mis-assembled by K-means Clustering

############### running K-means clustering ################
Main Script is "Run_Kmeans-clustering.sh"
Before running apply command
	chmod a+x Run_Kmeans-clustering.sh
running the script
	./Run_Kmeans-clustering.sh <fasta format file> <sam format file without headers> <number of threads>

Inside the "Run_Kmeans-clustering.sh" shell script, set the paths as described.

OUTPUT: results_with_groups
output file consists of contig_id, length of contig, group assigned "1 or 2" from length set.

################# running REAPR ####################
After running K-means clustering, run REAPR separately
The supporting script for REAPR's running is also given, namely, "reapr-steps.sh"
	User must define the required paths inside this script

	Running "reapr-steps.sh"
	chmod a+x reapr-steps.sh
	./reapr-steps.sh <fasta-file> <reads1> <reads2> <output_string> <number of processors>
OUTPUT: 05.summary.stats.tsv
	gives the error count and details predicted for each contig
	"reapr_with_error_contigs" : file with contigs_id classified as mis-assembled even with single base error predicted by REAPR
	"reapr_without_error_contigs" : file with contigs_id classified as correctly assembled even without any error predicted by REAPR


##################### Combining outputs from REAPR and K-means #####################################

perl combine-reapr-kmeans.pl <path to output_string of REAPR> >results_with_groups_reapr_class


############################################################################################################
###Note
	At the end, the section marked after "kmeans clustering along with predefined positive and negative instances" is optional.
	Already known some of the instances inside "known_instances" are also used during clustering, which may help the user to defile the clusters "1 or 2" as "correctly assembled or mis-assembled". K-means clustering reports two clusters marked as 1 or 2 for each length set of contigs without label as "correctly assembled or mis-assembled".
	output: results_with_known_groups
	output file consists of contig_id, length of contig, group assigned "1 or 2 " from length set, along with positive or negative as from known instances of contigs


######################################################################

Two files consisting of fasta format file for correctly assembled contigs set (positive.fa) and mis-assembled contigs set (negative.fa) as sample files

