```python
def ask(god, question):
    """Ask the user to input the response of a god to a question."""
    print(f"\nAsking {god}: {question}")
    response = input(f"What does {god} say? (da/ja): ").strip().lower()
    while response not in ['da', 'ja']:
        print("Invalid response. Please answer with 'da' or 'ja'.")
        response = input(f"What does {god} say? (da/ja): ").strip().lower()
    return response

def translate(response, meaning):
    """Translate 'da' or 'ja' into True or False based on their meaning."""
    if meaning == 'da_means_yes':
        return response == 'da'
    else:
        return response == 'ja'

def determine_roles():
    """Determine the roles of each god using logical questions."""
    print("Welcome to the Three Gods Riddle!")
    print("You are the gods A, B, and C. One is True (always tells the truth), one is False (always lies), and one is Random (answers randomly).")
    print("The gods answer with 'da' or 'ja', which may mean 'yes' or 'no' in their language.\n")

    # Step 1: Determine if A is Random
    # Ask god A the same question twice to check consistency
    response_A1 = ask('A', "If I asked you 'Is 2 plus 2 equal to 4?', would you say 'da'?")
    response_A2 = ask('A', "If I asked you 'Is 2 plus 2 equal to 4?', would you say 'da'?")

    if response_A1 != response_A2:
        # A is Random because their answers are inconsistent
        random_god = 'A'
        not_random_gods = ['B', 'C']
        # Since A is Random, we need to determine the meaning of 'da' and 'ja' using B or C
        # Ask god B: "If I asked you 'Is 2 plus 2 equal to 4?', would you say 'da'?"
        response_B = ask('B', "If I asked you 'Is 2 plus 2 equal to 4?', would you say 'da'?")
        if response_B == 'da':
            meaning = 'da_means_yes'
        else:
            meaning = 'da_means_no'
    else:
        # A is not Random (could be True or False)
        # Determine the meaning of 'da' and 'ja' based on A's consistent answer
        if response_A1 == 'da':
            meaning = 'da_means_yes'
        else:
            meaning = 'da_means_no'

        # Step 2: Determine if B is Random
        # Ask god A: "If I asked you 'Is B the Random god?', would you say 'da'?"
        response_A_B = ask('A', "If I asked you 'Is B the Random god?', would you say 'da'?")
        is_B_random = translate(response_A_B, meaning)

        if is_B_random:
            # B is Random
            random_god = 'B'
            not_random_gods = ['A', 'C']
        else:
            # C is Random
            random_god = 'C'
            not_random_gods = ['A', 'B']

    # Step 3: Determine the roles of the non-Random gods
    # Ask one of the non-Random gods: "If I asked you 'Is A the True god?', would you say 'da'?"
    response_not_random = ask(not_random_gods[0], f"If I asked you 'Is {not_random_gods[1]} the True god?', would you say 'da'?")

    # Translate the response based on the meaning of 'da' and 'ja'
    is_other_true = translate(response_not_random, meaning)

    if is_other_true:
        true_god = not_random_gods[1]
        false_god = not_random_gods[0]
    else:
        true_god = not_random_gods[0]
        false_god = not_random_gods[1]

    # Assign roles
    assignments = {
        true_god: 'True',
        false_god: 'False',
        random_god: 'Random',
    }

    return assignments

# Solve the riddle
solution = determine_roles()
print("\nSolution:", solution)
```
