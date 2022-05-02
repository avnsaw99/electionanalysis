# Election Analysis
Code: Python

## Project Overview

Step 1:

Initialize a county list, like the candidate_options list, that will hold the names of the counties.
Initialize a dictionary, like the candidate_votes dictionary, that will hold the county as the key and the votes cast for each county as the values.

Step 2:

Initialize an empty string, like winning_candidate, that will hold the county name for the county with the largest turnout.
Initialize a variable, like the winning_count variable, that will hold the number of votes of the county that had the largest turnout.

Step 3:

While reading the election results from each row inside the for loop, write a script that gets the county name from each row.

Step 4a:

Write a decision statement with a logical operator to check if the county name acquired in Step 3 is in the county list you created in Step 1.

Step 4b:

If the county is not in the list created in Step 1, add it to the list of county names like you did when adding a candidate to the candidate_options list.
Step 4c:

Write a script that initializes the county vote to zero, like you did when you began to track the vote counts for the candidates.

Step 5:

Write a script that adds a vote to the county’s vote count as you are looping through all the rows, like you did for the candidate’s vote count.

Step 6a:

Write a repetition statement to get the county from the county dictionary that was created in Step 1.

Step 6b:

Initialize a variable to hold the county’s votes as they are retrieved from the county votes dictionary.

Step 6c:

Write a script that calculates the county’s votes as a percentage of the total votes.

Step 6d:

Write a print statement that prints the current county, its percentage of the total votes, and its total votes to the command line.

Step 6e:

Write a script that saves each county, the county’s total votes, and the county’s percentage of total votes to the election_results.txt file.

Step 6f:

Write a decision statement that determines the county with the largest vote count and then adds that county and its vote count to the variables created in Step 2.

Step 7:

Write a print statement that prints out the county with the largest turnout.

Step 8:

Write a script that saves the county with the largest turnout to the election_results.txt file.

## Resources

CSV file: election_results.csv

VS Code: Python 3.7.6

## Results

![Python Code](https://user-images.githubusercontent.com/15044088/166174768-1729d73a-6c5b-4609-9f54-243ee3079a98.PNG)

    # -*- coding: UTF-8 -*-
    """PyPoll Homework Challenge Solution."""

    # Add our dependencies.
     import csv
     import os

    # Add a variable to load a file from a path.
      file_to_load = os.path.join("Resources", "election_results.csv")
    # Add a variable to save the file to a path.
      file_to_save = os.path.join("analysis", "election_analysis.txt")

    # Initialize a total vote counter.
     total_votes = 0

    # Candidate Options and candidate votes.
       candidate_options = []
       candidate_votes = {}

     # 1: Create a county list and county votes dictionary.
          county_options = []
          county_votes = {}

       # Track the winning candidate, vote count and percentage
            winning_candidate = ""
             winning_count = 0
             winning_percentage = 0

        # 2: Track the largest county and county voter turnout.
          winning_county = ""
           winning_county_count = 0
            winning_county_percentage = 0

 
          # Read the csv and convert it into a list of dictionaries
            with open(file_to_load) as election_data:
          reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_options:

            # 4b: Add the existing county to the list of counties.
            county_options.append(county_name)


            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1


     # Save the results to our text file.
         with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county_name in county_votes:

        # 6b: Retrieve the county vote count.
        votes = county_votes[county_name]

        # 6c: Calculate the percentage of votes for the county.
        vote_percentage = float(votes) / float(total_votes) * 100


        # 6d: Print the county results to the terminal.
        county_results = (
            f"{county_name}: {vote_percentage:.1f}% ({votes:,})\n")
        print(county_results)

        # 6e: Save the county votes to a text file.
        txt_file.write(county_results)

        # 6f: Write an if statement to determine the winning county and get its vote count.
        if (votes > winning_county_count) and (vote_percentage > winning_county_percentage):

            winning_county_count = votes
            winning_county = county_name
            winning_county_percentage = vote_percentage

    # 7: Print the county with the largest turnout to the terminal.

    largest_county_turnout = (
        f"\n-------------------------\n"
        f"Largest County Turnout: {winning_county}\n"
        f"-------------------------\n")
    print(largest_county_turnout)


    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_turnout)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)

## Personal Problems

I think logically all programming languages maintain a certain uniformity. We go in, and go to the drawing board, brainstorm our ideas and then build up the logic over how to go about. However where it gets further complex is the syntax of each new language and the need to be able to write code that is perfect and flawless for the terminal to run the code.

I personally struggled this time with Python, because it was a completely alien concept. VBA atleast had a certain level of familiarity due to the nature of my work, however there was zero exposure to python. The idea that you link a CSV file through your code into python and scrub that data was something I had heard of, but never done myself. With Excel and VBA, I was not highly dependent on the module for corect answers. I was highly dependent on the module this time, to be able to understand things. Overall I feel like even though things eventually became easier while following along the module correctly, this new language was definitely daunting, but exciting. 


