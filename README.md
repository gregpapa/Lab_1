# Εργαστήριο 1 -- Αρχιτεκτονική Υπολογιστών

**1.** _Βασικά χαρακτηριστικά συστήματος από το αρχείο starter_se.py_:  
 * .cache_line size: 64bytes
 * .voltage domain: 3.3V
 * .clock domain: 1GHz
 * .type of CPU: minor 
 * .CPU_frequency: 4GHz
 * .number of cores: 1
 * .CPU_voltage: 1.2V
 * .memory type: DDR3_1600_8x8
 * .memory channels: 2
 * .memory ranks per channel: none
 * .memory size: 2GB
	
**2.** 
* _α)_
    
        config.ini:
        [system]
        cache_line_size=64
	
	    [system.voltage_domain]
	    voltage=3.3
	
	    [system.clk_domain]
	    clock=1000
	
	    [system.cpu_cluster.cpus]
	    type=MinorCPU
	
	    [system.cpu_cluster.clk_domain]
	    clock=250
	
	    [system.cpu_cluster.cpus]
	    numThreads=1
	
	    [system.cpu_cluster.voltage_domain]
	    voltage=1.2
	
	    [system.mem_ctrls0.dram] και αντίστοιχα [system.mem_ctrls1.dram]
	    type=DRAMInterface
	    banks_per_rank=8
	    devices_per_rank=8
	    ranks_per_channel=2
	    range=0:2147483648

 * _b)_**stats.txt: system.cpu_cluster.cpus.committedInsts --> 5028**
    Στο αρχείο config.ini παρουσιάζονται οι εντολές που χρειάζονται για να προσομοιωθεί το σύστημα,ενώ στο αρχείο stats.txt παρουσιάζονται ο αριθμός των συνολικών εντολών που χρειάστηκαν για ολοκληρωθεί το πρόγραμμα "Hello World".
    
 * _c)_**stats.txt: system.cpu_cluster.l2.overall_accesses::total --> 479**
	Προσπελάσεις L2 cache:
	Αν δεν παρεχόταν από τον εξομοιωτή οι προσπελάσεις της L2 Cache, 
	θα μπορούσαμε να τις υπολογίσουμε προσθέτοντας τα misses της icache(332) 
	και dcache (179).

3. **SimpleCPU**:
	Είναι ένα λειτουργικό, in-order μοντέλο, το οποίο είναι κατάλληλο  
    για περιπτώσεις όπου δεν είναι απαραίτητο ένα λεπτομερές μοντέλο. Περιλαμβάνει
	περιόδους προθέρμανσης, συστήματα πελατών που οδηγούν έναν κεντρικό υπολογιστή
	ή ελέγχους για το αν λειτουργεί ένα πρόγραμμα. Προκειμένου να υποστηρίξει 
	το νέο σύστημα μνήμης, ξαναγράφτηκε και αποτελείται από τις παρακάτω κατηγορίες:
	
   **BaseSimpleCPU**:
	Η συγκεκριμένη έκδοση δεν μπορεί να εκτελεστεί μόνη της.
	Πρέπει να εκτελεστεί μια από τις κλάσεις που κληρονομεί μια από τις
	κλάσεις που κληρονομεί από το BaseSimpleCPU, είτε το AtomicSimpleCPU
	ή το TimingSimpleCPU.

   **AtomicSimpleCPU**:
	Χρησιμοποιεί προσβάσεις ατομικής μνήμης καθώς και τις καθυστερήσεις
	που έχουν υπολογιστεί στις ατομικές προσβάσεις, για να εκτιμήσει 
	το συνολικό χρόνο πρόσβασης στην cache.
   
   **TimingSimpleCPU**:
	Χρησιμοποιεί προσβάσεις μνήμης χρονισμού. Σταματά στις cache accesses, 
	και περιμένει να ανταποκριθεί το σύστημα μνήμης, πριν προχωρήσει.
	Οι δύο τελευταίες εκδόσεις προέρχονται από το BaseSimpleCPU και εφαρμόζουν το 
	ίδιο σύνολο λειτουργιών. 

   **MinorCPU**:
	Είναι ένα in-order μοντέλο επεξεργαστή με δυνατότητα pipelining. Είναι 
	πιο περίπλοκο στην αρχιτεκτονική του και βοηθά στο να προσομοιώνουμε 
	την μικρο-αρχιτεκτονική επεξεργαστών με αντίστοιχες δυνατότητες. 

  * _a)_ **TimingSimpleCPU**:
        final_tick 39989000                    
        host_seconds 0.05
        sim_seconds 0.000040  
      
	**MinorCpu**: 
		final_tick: final_tick 34104000
		host_seconds 0.13
		sim_seconds 0.000034 
   
  * _b)_ Eπειδή το TimingSimpleCPU δεν χρησιμοποιεί pipeline είναι πιο αργό από το minor, κάτι που φαίνεται και στους χρόνους.
	
  * _c)_ Αλλαγή στο cpu-clock=1GHz:
		
	-**TimingSimpleCPU**: 
	final_ticks 54995000
	host_seconds 0.03
	sim_seconds 0.000055
	
	-**MinorCPU**:
	final_ticks 41480000
	host_seconds 0.15
	sim_seconds 0.000041
	Παρατηρούμε πως μειώνοντας το cpu-clock αυξάνεται ο χρόνος εκτέλεσης.	

* Αλλαγή στην συχνότητα λειτουργίας συστήματος=500MHz:
	
	-**TimingSimpleCPU**: final_ticks 46159000
			  host_seconds 0.06
			  sim_seconds 0.000046
	
    -**MinorCPU**: final_ticks  40024000
		   host_seconds 0.23
		   sim_seconds 0.000040
	Παρατηρούμε πως με την μείωση της συχνότητας λειτουργίας συστήματος, αυξάνεται ο χρόνος εκτέλεσης.
      
* Αλλαγή στην τεχνολογία μνήμης = DDR3_2133_8x8 :
	
	-**TimingSimpleCPU**: 
            final_tick 38458000
			   host_seconds 0.04
			   sim_seconds 0.000038
	
    -**MinorCPU**: final_tick 32931000
		   host_seconds 0.14
		   sim_seconds 0.000033
	Παρατηρούμε πως σε σχέση με την DDR3_1600_8x8, η DDR3_2133_8x8 είναι πιο γρήγορη.
