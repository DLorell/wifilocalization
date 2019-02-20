# wifilocalization

Our algorithm is an amalgamation of the formula provided by the paper found here
https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ez-mobicom.pdf
and our own intition. 

For the data processing:
    First we seperate the traces data set according to car.
    
    Then we seperate the traces within the car's subset into individual legs of the
    triangles.
    
    Then we calculate the mean squared difference of each rss, loc_x, and loc_y
    datapoint accross all traces within a leg.
    
    Then we cluster the traces with that information, using an iterative and
    exhaustive approach, which while it won't scale, this project was sufficiently small.
    The clusters are determined by groups of traces within the leg which are within a parameter
    of squared distance. This parameter is determined algorithmically by a function which
    takes a percentage and increases the clustering parameter until it has produced the tightest cluster 
    containing X percent of the data. We aimed to keep 30% of the data per leg.
    
    We take the cluster and summarize it into a single trace.
    
    We now have a clean trace per leg per device.
    

For the algorithm:
     We follow equation 3 in the paper and estimate gamme to be
2.2 for a largely empty space (wikipedia relates it to a retail store
https://en.wikipedia.org/wiki/Log-distance_path_loss_model). The paper then describes
the transmit power, or P, as "the RSS from the the AP at a distance of one meter". We
assumed relatively similar transmit power across all APs in use, and used our knowledge
of the location of AP with MAC address 44:91:60:d3:d6:94 to look at a large set of 
RSS values measured at a length of 0.95-1.05 meters away. Taking the average of this set
gave us our estimation of P. We could then use the equation for any RSS value.

Once we could easily find the potential distance at which the AP would be found from any
given point measured, we used GPS-inspired circle intersection to find the AP location.
Essentially, we look at each of the nine traces, find the point on that trace with the storngest
RSS, select three points from those nine (so each will be on a different leg of the triangle)
and find the intersection of the circles with center at those points and radius given by our
RSS-to-distance function.

Unfortunately, we were unable to get the circle intersection code working before the deadline
