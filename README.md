"""
Developer: Nakealius Ards
Date: 24 Jan 2026
Purpose: This code uses a data structure to display, sort,
and update a List of U.S. States containing the state capital,
overall state population, and state flower.
"""
import sys
from dataclasses import dataclass
from PIL import Image
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker

def welcome_prompt():
    """Call to the welcome prompt function"""
    print("Welcome to Lab 3 State Information Application! This application can provide "
          "you with the state capital, state population and the state flower. Please select "
          "from the below menu to begin: \n")

def exit_prompt():
    print("Thank you for using the Lab 3 State Information Application!")

def continue_prompt():
    """Call to the continue prompt function"""
    while True:
        continue_input = input("\nWould you like to return to the main menu? (Yes or No): ").lower()
        if continue_input == "yes":
            main_menu() # Return to main menu
            break
        if continue_input == "no":
            exit_prompt()
            sys.exit() # Exit the application
        else:
            print("Invalid Input. Please try again .")

@dataclass
class State:
    """Create an instance of the state that is being called"""
    state_name: str
    capital: str
    population: int
    flower: str
    def __init__(self, state_name, capital, population, flower):
        self.state_name = state_name
        self.capital = capital
        self.population = population
        self.flower = flower


def display_data(state):
    """Display and format the state data"""
    print(f"Please see below for the State of {state.state_name}'s information:\n"
            f"State Capital: {state.capital}\n"
            f"State Population: {state.population:,}\n"
            f"State Flower: {state.flower}")

# Store the list of state abbreviations for selection screens
state_list = ["AL - Alabama", "AK - Alaska", "AZ - Arizona", "AR - Arkansas",
              "CA - California", "CO - Colorado", "CT - Connecticut", "DE - Delaware",
              "FL - Florida", "GA - Georgia", "HI - Hawaii", "ID - Idaho", "IL - Illinois",
              "IN - Indiana", "IA - Iowa", "KS - Kansas", "KY - Kentucky", "LA - Louisiana",
              "ME - Maine", "MD - Maryland", "MA - Massachusetts", "MI - Michigan",
              "MN - Minnesota", "MS - Mississippi", "MO - Missouri", "MT - Montana",
              "NE - Nebraska", "NV - Nevada", "NH - New Hampshire", "NJ - New Jersey",
              "NM - New Mexico", "NY - New York", "NC - North Carolina", "ND - North Dakota",
              "OH - Ohio", "OK - Oklahoma", "OR - Oregon", "PA - Pennsylvania",
              "RI - Rhode Island", "SC - South Carolina", "SD - South Dakota", "TN - Tennessee",
              "TX - Texas", "UT - Utah", "VT - Vermont", "VA - Virginia", "WA - Washington",
              "WV - West Virginia", "WI - Wisconsin", "WY - Wyoming"]

def state_storage():
    """Store information about the state in a dictionary data structure"""
    # Population must be an int to avoid fail on a changed population * fixed
    # COMMUNITY TIP: Update population values here periodically.
    # Ensure population is stored as an integer for consistency.
    states = {
    "AL": ("Alabama", "Montgomery", 5237750, "Camellia"),
    "AK": ("Alaska", "Juneau", 747379, "Forget-me-not"),
    "AZ": ("Arizona", "Phoenix", 7801100, "Saguaro Blossom"),
    "AR": ("Arkansas", "Little Rock", 3126140, "Apple Blossom"),
    "CA": ("California", "Sacramento", 39896400, "California Poppy"),
    "CO": ("Colorado", "Denver", 6069800, "Rocky Mountain Columbine"),
    "CT": ("Connecticut", "Hartford", 3739160, "Mountain Laurel"),
    "DE": ("Delaware", "Dover", 1082900, "Peach Blossom"),
    "FL": ("Florida", "Tallahassee", 24306900, "Orange Blossom"),
    "GA": ("Georgia", "Atlanta", 11413800, "Cherokee Rose"),
    "HI": ("Hawaii", "Honolulu", 1455660, "Hibiscus"),
    "ID": ("Idaho", "Boise", 2062610, "Syringa"),
    "IL": ("Illinois", "Springfield", 12846000, "Violet"),
    "IN": ("Indiana", "Indianapolis", 7012560, "Peony"),
    "IA": ("Iowa", "Des Moines", 3287640, "Wild Rose"),
    "KS": ("Kansas", "Topeka", 3008820, "Sunflower"),
    "KY": ("Kentucky", "Frankfort", 4663930, "Goldenrod"),
    "LA": ("Louisiana", "Baton Rouge", 4617080, "Magnolia"),
    "ME": ("Maine", "Augusta", 1415740, "White Pine Cone & Tassel"),
    "MD": ("Maryland", "Annapolis", 6355540, "Black-eyed Susan"),
    "MA": ("Massachusetts", "Boston", 7275380, "Mayflower"),
    "MI": ("Michigan", "Lansing", 10254700, "Apple Blossom"),
    "MN": ("Minnesota", "St. Paul", 5873360, "Pink & White Lady's Slipper"),
    "MS": ("Mississippi", "Jackson", 2942790, "Magnolia"),
    "MO": ("Missouri", "Jefferson City", 6320320, "Hawthorn"),
    "MT": ("Montana", "Helena", 1149100, "Bitterroot"),
    "NE": ("Nebraska", "Lincoln", 2040670, "Goldenrod"),
    "NV": ("Nevada", "Carson City", 3373680, "Sagebrush"),
    "NH": ("New Hampshire", "Concord", 1422700, "Purple Lilac"),
    "NJ": ("New Jersey", "Trenton", 9743270, "Violet"),
    "NM": ("New Mexico", "Santa Fe", 2148440, "Yucca"),
    "NY": ("New York", "Albany", 20127000, "Rose"),
    "NC": ("North Carolina", "Raleigh", 11375700, "Dogwood"),
    "ND": ("North Dakota", "Bismarck", 811610, "Wild Prairie Rose"),
    "OH": ("Ohio", "Columbus", 12001800, "Scarlet Carnation"),
    "OK": ("Oklahoma", "Oklahoma City", 4158420, "Oklahoma Rose"),
    "OR": ("Oregon", "Salem", 4309810, "Oregon Grape"),
    "PA": ("Pennsylvania", "Harrisburg", 13200800, "Mountain Laurel"),
    "RI": ("Rhode Island", "Providence", 1130070, "Violet"),
    "SC": ("South Carolina", "Columbia", 5660830, "Yellow Jessamine"),
    "SD": ("South Dakota", "Pierre", 937397, "Pasque Flower"),
    "TN": ("Tennessee", "Nashville", 7386640, "Iris"),
    "TX": ("Texas", "Austin", 32416700, "Bluebonnet"),
    "UT": ("Utah", "Salt Lake City", 3624400, "Sego Lily"),
    "VT": ("Vermont", "Montpelier", 648063, "Red Clover"),
    "VA": ("Virginia", "Richmond", 8964220, "American Dogwood"),
    "WA": ("Washington", "Olympia", 8159900, "Western Rhododendron"),
    "WV": ("West Virginia", "Charleston", 1768950, "Rhododendron"),
    "WI": ("Wisconsin", "Madison", 6022120, "Wood Violet"),
    "WY": ("Wyoming", "Cheyenne", 592720, "Indian Paintbrush")
    }
    # Return the abbreviation state object
    return {abbr: State(*info) for abbr, info in states.items()}

def state_flower_img():
    """Populate an image based on the selected state abbreviation"""
    # COMMUNITY TIP: Place all flower images in assets/images/.
    # Use consistent naming conventions for easier maintenance.
    state_flower_imgs = {
        "AL": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "AL_camellia-flower.jpg")),
        "AK": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "AK_Alpineforgetmenot.jpg")),
        "AZ": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "AZ_saguaroflowersFlickr.jpg")),
        "AR": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "AR_AppletreeblossomArkansasflower.JPG")),
        "CA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "CA_flowerCaliforniaPoppy.jpg")),
        "CO": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "CO_Colorado_columbine2.jpg")),
        "CT": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "CT_Mountain-Laural-flowers2.jpg")),
        "DE": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "DE_peachblossomspeachflowers.jpg")),
        "FL": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "FL_OrangeBlossomsFloridaFlower.jpg")),
        "GA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "GA_CherokeeRoseFlower.jpg")),
        "HI": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "HI_yellowhibiscusPuaAloalo.jpg")),
        "ID": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "ID_syringaPhiladelphuslewisiiflower.jpg")),
        "IL": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "IL_singlebluevioletflower.jpg")),
        "IN": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "IN_PeonyPaeoniaflowers.jpg")),
        "IA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "IA_WildPrairieRose.jpg")),
        "KS": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "KS_native-sunflowers.jpg")),
        "KY": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "KY_stateflowergoldenrod-bloom.jpg")),
        "LA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "LA_magnoliagrandifloraMagnoliaflower.jpg")),
        "ME": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "ME_whitepinemalecones.jpg")),
        "MD": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MD_FlowerMDBlack-eyedSusan.jpg")),
        "MA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MA_MayflowerTrailingArbutus.jpg")),
        "MI": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MI_appleblossombeauty.jpg")),
        "MN": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MN_pinkwhiteladysslipperflower1.jpg")),
        "MS": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MS_magnoliablossomflower01.jpg")),
        "MO": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MO_hawthornflowersblossoms1.jpg")),
        "MT": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "MT_bitterrootfloweremblem.jpg")),
        "NE": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NE_goldenrodflowersyellow4.jpg")),
        "NV": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NV_Nevada-Sagebrush-Artemisia-tridentata.jpg")),
        "NH": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NH_lilacflowerspurplelilac.jpg")),
        "NJ": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NJ_wood-violet.jpg")),
        "NM": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NM_YuccaFlowersclose.jpg")),
        "NY": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NY_redrosebeautystateflowerNY.jpg")),
        "NC": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "NC_floweringdogwoodflowers2.jpg")),
        "ND": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "ND_flowerwildprairierose.jpg")),
        "OH": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "OH_redcarnationOhioflower.jpg")),
        "OK": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "OK_Oklahoma-Rose-Tea-rose.jpg")),
        "OR": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "OR_Oregongrapeflowers2.jpg")),
        "PA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "PA_Mt_Laurel_Kalmia_Latifolia.jpg")),
        "RI": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "RI_violetsflowers.jpg")),
        "SC": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "SC_CarolinaYellowJessamine101.jpg")),
        "SD": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "SD_Pasqueflower-03.jpg")),
        "TN": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "TN_purpleirisflower.jpg")),
        "TX": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "TX_BluebonnetsBlueRed.jpg")),
        "UT": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "UT_SegoLily.jpg")),
        "VT": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "VT_redcloverstateflowerWV.jpg")),
        "VA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "VA_floweringDogwoodSpring.jpg")),
        "WA": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "WA_flower_rhododendronWeb.jpg")),
        "WV": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "WV_rhododendronWVstateflower.jpg")),
        "WI": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "WI_wood-violet.jpg")),
        "WY": np.asarray(Image.open("Week 3 Assignment Image Files/"
                                    "WY_indianpaintbrushWYflower.jpg"))
    }
    return state_flower_imgs

def sort_states(states):
    """Sort the list of states from the state storage method"""
    # Sort by the state name, then capital, then population, then finally the flower
    sorted_states = sorted(states, key=lambda s: (s.state_name, s.capital, s.population, s.flower))

    state_order = 1 # Counter for the state order sorted
    for state in sorted_states: # Get values of sorted_states and return them in a string format
        print(f"\n{state_order}.\nState Name: {state.state_name}\n"
              f"State Capital: {state.capital}\n"
              f"State Population: {state.population:,}\n"
              f"State Flower: {state.flower}\n")
        state_order += 1 # Increase the state order by 1 each iteration

def sorted_population(states):
    """Sort the list of states from the state storage method"""
    # COMMUNITY TIP: Extend this function to show top 10 states or export data to CSV.
    state_names = []
    state_population = []
    # Convert population to an integer and return sorted list in descending order
    descending_sort = sorted(states, key=lambda s: int(s.population), reverse=True)
    state_order = 1
    # Check for descending sorting
    for state in descending_sort:
        # Prints the first 5 items in the descending sort list
        state_names.append(state.state_name)
        state_population.append(int(state.population))
        print((f"\n{state_order}.\n"
              f"State Name: {state.state_name}\n"
              f"State Capital: {state.capital}\n"
              f"State Population: {state.population:,}\n"
              f"State Flower: {state.flower}\n"))
        state_order += 1 # Count the number of printed states
        if state_order > 5: # Stops printing at 5
            #print(list(state_names)) # Check order of list
            #print(list(state_population)) # Check order of list
            # Reverse the list to appear in ascending order
            state_names_reversed = list(reversed(list(state_names)))
            state_population_reversed = list(reversed(list(state_population)))
            # Create a bar graph with the top 5 states
            plt.figure(figsize=(9, 3)) # Size graph
            # Set the state names on the x and the population on the y.
            plt.bar(state_names_reversed, state_population_reversed)
            plt.suptitle("Top 5 Populated States Bar Graph")
            # Format the yaxis to avoid numerical values only increasing by 0.5
            plt.gca().yaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f"{int(x):,}"))
            plt.show()
            break

def update_population():
    """Update the population based on the user's input"""
    while True:
        user_d_input = input("\nEnter the state abbreviation of your choice: ").upper()
        if user_d_input not in ss:
            print("Invalid Input! Please enter a valid state abbreviation.")
            continue

        try:
            if user_d_input in ss:
                new_population = int(input("Enter the new population: "))
                if new_population > 0:  # Check for positive numbers
                    ss[user_d_input].population = new_population
                    print(f"The population for {user_d_input} has been "
                          f"updated to: {new_population}\n")
                    break

                print("Invalid Input! Please enter a positive number"
                      " to update the population.")
        except ValueError:
            print("Invalid Input! Please enter a positive numerical number"
                  " to update the population.")

def main_menu():
    """Provide a selection menu to the user"""
    welcome_prompt()

    while True:
        # Display menu prompt for the user
        menu_prompt = input("Please select from the below menu to continue: \n"
                            "A) Display all states in alphabetical order\n"
                            "B) Search for a specific state's name, population, "
                            "and flower image\n"
                            "C) View a Bar graph of the top 5 populated States"
                            " showing their overall population\n"
                            "D) Update the overall state population for a specific state\n"
                            "E) Exit the program\n").upper()
        # Call sort_states and return all values as a list
        if menu_prompt == "A":
            sort_states(list(ss.values()))
            continue_prompt()
            break

        # Matches the user input to the abbreviation in the
        # sort_states() method and return the associated object
        if menu_prompt == "B":
            while True:
                try:
                    print("Please choose the specific state information "
                            "you are looking for: \n")
                    for state in state_list:
                        print(state, end="\n")
                    user_b_input = input("\nEnter the state abbreviation "
                                            "of your choice: ").upper()
                    print("\n")
                    # Get the user input and traverse the sorted state method for the dictionary
                    # data associated with the selected abbreviation and
                    # display the information stored
                    if user_b_input in ss:
                        chosen_state = ss[user_b_input]
                        display_data(chosen_state)
                    # Traverse the state flower image method for the dictionary
                    # data associated with the selected abbreviation and display the flower
                    if user_b_input in flower_images:
                        plt.imshow(flower_images[user_b_input])
                        plt.show()
                        continue_prompt()
                        break

                    print("Invalid Input! Please enter a valid state abbreviation.")
                except ValueError:
                    print("Invalid Input! Please enter a valid state abbreviation.")

        # Top 5 populated cities using a bar graph
        if menu_prompt == "C":
            # Sort the population by calling sorted_population() method
            sorted_population(list(ss.values()))
            continue_prompt()
            break

        # Update the state populations
        if menu_prompt == "D":
            update_population()
            continue_prompt()

        # Exit the application
        if menu_prompt == "E": # Exit the program
            exit_prompt()
            sys.exit()

        print("Invalid Input! Please select from the below menu and try again.\n")

ss = state_storage()
flower_images = state_flower_img()

def main():
    """Main function"""
    main_menu()

main()

# TODO (Community Ideas):
# - Add search by capital city
# - Export state data to JSON/CSV
# - Add GUI interface with Tkinter or PyQt
# - Integrate live census data updates
