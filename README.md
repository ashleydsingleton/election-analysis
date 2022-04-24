# Election Analysis Project

## Overview of Election-Audit
#### A Colorado Board of Elections employee has given us the following tasks to complete the election audit of a recent local congressional election.
1. Calculate the total number of votes cast. 
2. Get the complete list of county votes.
3. Calculate the total number of votes by county.
4. Calculate the percentage of representation by county.
5. Determine the county with the largest voter turnout.
6. Get a complete list of candidates who received votes.
7. Calculate the total number of votes each candidate received.
8. Calculate the percentage of votes each candidate won.
9. Determine the winner of the election based on popular vote.

## Resources
- Data Source: [election_results_2.csv](https://github.com/ashleydsingleton/election-analysis/files/7794077/election_results_2.csv)
- Software: Python 3.7.6, Visual Studio Code, 1.62

## Election-Audit Results
The analysis of the election results show:
- There were 369,711 votes cast in the election.
- The votes and percentages by county were:
  - County Votes:
      - Jefferson: 10.5% (38,855)
      - Denver: 82.8% (306,055)
      - Arapahoe: 6.7% (24,801)
#### - The county with the largest voter turnout was Denver.
- The candidates were:
  - Charles Casper Stockham
  - Diana DeGette
  - Raymon Anthony Doane
- The candidate results were:
  - Charles Casper Stockham received 23.0% of the vote and 85,213 number of votes.
  - Diana DeGette received 73.8% of the vote and 272,892 number of votes.
  - Raymon Anthony Doane received 3.1% of the vote and 11,606 number of votes.
#### - The winner of the election was: 
####      Diana DeGette, who received 73.8% of the vote and 272,892 number of votes.

### The Election Results Printed to the Command Line:
Using repetition statements, conditional statements with logical operators, and print statements, print out the candidate and county election results to the command line.
#### Terminal Results Screenshot - 
![image](https://user-images.githubusercontent.com/92938054/147777381-91be2392-bef2-437e-95d8-b330e5d19392.png)

### The Election Results Saved to a Text File:
Using your knowledge of writing data to a text file, write the winning candidate results and the county election results to the election_analysis_2.txt file.
#### Click the following link to view the text file:
[election_analysis_2.txt](https://github.com/ashleydsingleton/election-analysis/files/7794100/election_analysis_2.txt)
#### Text File Results Screenshot - 
![Text File screenshot](https://user-images.githubusercontent.com/92938054/147777753-b345fca1-5afa-46f1-8306-a37d77c04d9f.png)

### PyPoll_Challenge Code:
```
# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results_2.csv")
# Assign a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis_2.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}

# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
winning_county = ""
winning_county_votes = 0
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
        if county_name not in county_list:

            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)

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
        ctyvotes = county_votes.get(county_name)
        ctyvotes_percentage = float(ctyvotes) / float(total_votes) * 100
        
        # 6c: Calculate the percentage of votes for the county.
        county_results = (
            f"{county_name}: {ctyvotes_percentage:.1f}% ({ctyvotes:,})\n")

         # 6d: Print the county results to the terminal.
        print(county_results)

         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)

         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (ctyvotes > winning_county_votes) and (ctyvotes_percentage > winning_county_percentage):
            winning_county_votes = ctyvotes
            winning_county = county_name
            winning_county_percentage = ctyvotes_percentage

    # 7: Print the county with the largest turnout to the terminal.
    winning_county_summary = (
        f"\n-------------------------\n"
        f"Largest County Turnout: {winning_county}\n"
        f"-------------------------\n\n")
    print(winning_county_summary)

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_summary)

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
        f"-------------------------\n\n")
    
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
```

## Election-Audit Summary
Using our current code, we were able to determine the requested data for the counties and the candidates participating in the election. This code tallied the total number of votes, calculated and analyzed the results showing the county with the largest voter turnout was Denver and the winning candidate was Diana DeGette who received 73.8% of the vote with a total of 272,892 votes. Since this code is not hard coded to print specific county and candidate names, but rather reads this information from the provided .csv file, it can be used for any election based on the information provided in the same format in the .csv file by simply updating the resource file information. If we were to have more columns of information in the .csv file to consider, for example districts or cities, then we could adjust the script accordingly with very minor changes. If there were more candidates in the file, the current code can handle that without any modifications. If the election was on a greater scale, say nationally, that change could be made very easily with a few modifications to the lines where we extract from the .csv file and with additional list and dictionary combinations. The election commission now has a flexible way to calculate and present election results quickly and accurately.
