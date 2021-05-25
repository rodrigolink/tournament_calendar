# tournament_calendar
Optimization of the Brazilian Football Championship with Genetic Algorithm

The Brazilian Football Championship is the main tournament of the most popular sport in Brazil. Most of the country is represented through one of the twenty teams (the Northern region was present in former editions). Brazil is a continent-sized country, so this tournament involves long trips, specially for teams in the Northeast region.

Starting with a set of conditions that will be discussed later, we will use Genetic Algorithms to propose a possible sequence of matches that minimize the impact of these long trips, although it is very clear there are teams who are geographically distant from the rest and there is no way to have every team with the same total distance. We try three different metrics for this optimization, all of them based on the distances traveled by each team.

We have the match sequence for each of the twenty teams for the 2020 Championship. There are two halves, where the second one repeats the order of the first, but flipping the home and away teams. Taking this into account, we only need to worry about the first half. We represent the matches for the first half with two tables, one with the opponents in each round and another telling us if the team plays at their home stadium or away (h or a).

We use two conidtions: 1) No team can play more than two rounds in a row at home or away; 2) The teams don’t return to their home city, going from one match to the location of the next. This new condition is only relevant to our evaluation metric. Without this condition, the total distance traveled by each team would be the sum of distance from their home cities to all the others. By adding this artificial condition, we are able to run our algorithm. The other option would be to develop a more complex metric.

We have three tables to work with. To get the optimum results, we should make changes in all of them at the same time, but this is rather difficult. Let’s tackle one at a time, leaving the other two fixed.

Order of teams – We are going to make changes in the order of the teams, keeping the matches and location tables fixed. In this case, our gene is a vector with the numbers from 1 to 20. The numbers represent each team in the alphabetical order, and the position in the vector represents the position in the generic list. That is, if the first number in the vector is 5, the fifth team in the alphabetical list will be ‘Team01‘. The mutation for each generation is the selection of two positions in the vector and swapping the teams.

Match location – We are going to make changes to the location table, keeping the order and matches tables fixed. That is, define the sequence of home and away matches for each team. It is important to respect the condition that no team can play more than two games in a row at home or away. In this case, our gene is a 20×19 matrix, where each row is the location of the matches for a team and each column is a round. The mutation is to pick a single match in the matches table and swap the home team. It is a valid mutation if the condition above is met.

Match order – We are going to make changes to the matches table, keeping the order of the teams fixed. That is, define the sequence of matches for each team. When we alter the sequence of the rounds, this also affects the location table. It is important to respect the condition that no team can play more than two games in a row at home or away. To simplify this case, our gene is a vector with numbers from 1 to 19, The numbers represent each round in the original order, and the position in the vector represents the position in the new order. That is, if the first number in the vector is 5, the fifth round in the original order will be considered the first round. The mutation for each generation is the selection of two positions in the vector and swapping the rounds. Again, it is important to respect the condition that no team can play more than two games in a row at home or away.

We have used geopy to get the coordinates of each city and haversine to calculate the distance between them.

We will try three different metrics, but they all depend on the vector with the total distance traveled by each team: 1)minimizing the total sum of the distance; 2) minimizing the difference between the smallest and the largest distances; 3) minimizing the standard deviation of the distribution of distances.

We have four parameters to define: 1) the number of genes (configurations) we will evaluate in each generation: 6; 2) the number of genes we will use to create the new generation: 2; 3) the number of generations: 500; 4) the original configurations. 

We weren´t able to create new configurations when running the Match order case.

Conclusion:
The Traveling Tournament Problem is really complex, and creating a competition with 20 teams is very hard. We split the problem in three tables and tried approaching the task one by one. Even so, we were able to move forward with two of them. We found that changing the order of the teams has a bigger impact than modifying the location of the games. In the former, we had a maximum distance traveled close to 54k kilometers, while in the latter it was close to 70k. This solution can be applied to any new Brazilian Championship. For the 2021 edition, we should update the list of teams and their cities, and then run the optimization of the order of the teams using the standard deviation as an evaluation metric.

Final Conclusion: we can generate an optimized tournament using genetic algorithms by modifying the order of the teams using standard deviation as an evaluation metric.
