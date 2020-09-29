# application.py 
Basketball-Stats-Tool
 Second project
 import constants
 import copy
 import sys

 teams_copy = copy.deepcopy(constants.TEAMS)
 players_copy = copy.deepcopy(constants.PLAYERS)
 player_data = []
 panthers = []
 bandits = []
 warriors = []
 teams = [panthers, bandits, warriors]
 num_players_team = len(players_copy) / len(teams_copy)

 def clean_data():
     for player in players_copy:
         player['height'] = int(player['height'].split()[0])
         if player['experience'] == 'YES':
             player['experience'] = True
         else:
             player['experience'] = False
         player_data.append(player)

 def balance_teams():
     for team in teams:
         while len(team) < num_players_team:
             team.append(player_data.pop())

 def display():
     print("----MENU----")
     print("Here are your choices: ")
     print("1. Display team stats")
     print("2. Quit")
     while True:
         try:
             display_quit_choice = input("Enter an option >")
             display_quit_choice = int(display_quit_choice)
             if display_quit_choice < 1 or display_quit_choice > 2:
                 print("Invalid choice. Please try again.")
                 continue
         except ValueError:
             print("Invalid choice. Please try again.")
             continue
         if display_quit_choice == 1:
             for index, team in enumerate(constants.TEAMS, 1):
                 print(f'{index}.{team}')
             while True:
                 try:
                     team_choice = input("Enter an option >")
                     if team_choice != '1' and team_choice != '2' and team_choice != '3':
                         raise ValueError("Invalid choice. Please try again.")
                     team_choice = int(team_choice)
                     break
                 except ValueError as err:
                     print(err)
             team_name = str(constants.TEAMS[team_choice-1])
             selected_team = teams[team_choice-1]
             player_list = [player['name'] for player in selected_team]
             def experience(selected_team):
                 experienced_players = []
                 for player in selected_team:
                     if player['experience']:
                         experienced_players.append(player)
                 return experienced_players
             def inexperience(selected_team):
                 inexperienced_players = []
                 for player in selected_team:
                     if player['experience'] == False:
                         inexperienced_players.append(player)
                 return inexperienced_players
             experienced_players = experience(selected_team)
             inexperienced_players = inexperience(selected_team)
             print(team_name + " stats")
             print("---------------")
             print("Total players: {}".format(len(selected_team)))
             print("Players on team: ")
             print(', '.join(player_list))
             print("Experienced players: {}".format(len(experienced_players)))
             print("Inexperienced players: {}".format(len(inexperienced_players)))
             continue_option = input("Press ENTER to continue...")
             if continue_option == '':
                 display()
             else:
                 print("Thank you for using our stats tool")
                 sys.exit()
         else:
             print("Thank you for using our stats tool")
             sys.exit()



 if __name__ == '__main__':
     clean_data()
     balance_teams()
     display()
