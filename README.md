#FIRST PART OF THE EXAM 
list_of_nucleotides = "ATGTCTGTCTGAA" #making it so the code can read the list of nucleotides
print(list_of_nucleotides)

list_of_nucleotides[0:2] #this will give us the first 2 in the list 
list_of_nucleotides[0+1:2+1] #since it is non-inclusive need to do +1 to make it inclusive 
list_of_nucleotides[0+2:2+2] #adding one to each thing to get another pair 


def generate_kmers(list_of_nucleotides, k): #defines the name of the function that will be done
    kmers = []
    for i in range(len(list_of_nucleotides) - k + 1): #the loop goes over the List_of_nucleotides, and then goes over each string in which a kmer can start 
        kmers.append(list_of_nucleotides[i:i+k]) #uses a substring withint the list_of_nucleotides, and then slices through it. Then adds it to the kmer list 
    return kmers #returns each list that the loop generates 

list_of_nucleotides = "ATGTCTGTCTGAA" #the sequence that the function reads through
k = 2 #assignes a vlaue of 2, for the length of each kmer 

kmers = generate_kmers(list_of_nucleotides, k) #makes the kmers variable the sequence of 2, meaning that it puts together the list_of_nucleotides sequence and and k 

print(kmers) #prints the list of kmers 



#SECOND PART OF THE EXAM

#1st function 

def single_sequence(filename, k):
  """
  This function will identify all substrings if k and subequent substrings, for a single sequence.
  Arguments:
   reads.fa: file containing sequence in FASTA form 
   k(int): the suze of the substrings 
   
  Returns:
    dict: Dictionary where substrings of size k and values are sets of subsequent substrings
  """

substrings = {} #where substings will be stored and also subsequent substrings 
with open(filename, 'r') as file: #makes sure file closes after being read 
  sequence = ''.join(line.strip() for line in file.readlines()[1:])  # skip the header line
  for i in range(len(sequence) - k + 1): #is a loop that will go over the sequence, making sure to stop where the size of k can be taken 
    substring = sequence[i:i+k] #taking k from the starting sequence
    subsequent_substring = sequence[i+k:i+2*k] #this will then take sunsequent string of k, following the first one
    if len(subsequent_substring) == k: #this line will check the length of the subsequent string, and make sure it is equal to k
      if substring in substrings:
        substrings[substring].add(subsequent_substring)
      else:
          substrings[substring] = {subsequent_substring} #this makes sure to check if the substring already exists, if not creates new value in dictionary 
return substrings #this will give all sequences and subsequent substrings 

#test function 
filename = 'reads.fa'
k = 3
substrings = single_sequence(filename, k)
print(substrings)


#3rd function 


def identify_smallest_k(filename):
    """
   This function will identify the smallest value of k
   
    Arguments:
    reads.fa: file containing sequence in FASTA form 

    Returns:
    int: The smallest value of k.
    """
    k = 1  #this line makes k equal to 1 
    while True: #because of the loop, this will stop the lopp when the statement is correct 
        all_substrings = all_sequences(filename, k)  # Using the function from before 
        unique_substrings_count = sum(len(substrings) for substrings in all_substrings.values()) #this will calculate the total unique substrings across all sequqnces within the file
        total_substrings_count = sum(len(substrings) * len(subsequent_substrings) 
                                     for substrings in all_substrings.values() 
                                     for subsequent_substrings in substrings.values()) #counts all substrings and subsequent substrings, acroos the FASTA file. It will then sum it all up 
        if unique_substrings_count == total_substrings_count: #will check the sum and make sure it is equal to unique substrings. If equal will mean there is only one possible subsequent substring 
            return k  #will give the value of k after the loop, (the smallest value of k possible)
        k += 1  #this will keep adding +1 to each loop of k, so it increaes as the loop goes

#test function:
filename = 'reads.fa'
smallest_k = identify_smallest_k(filename)
print("Smallest value of k:", smallest_k)

