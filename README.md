# Gurmanjot-and-Aditya-Submission-to-MicroHacks-Hackathon
# Bromine Budgeting App. Gurmanjot and Aditya Submission to MicroHacks Hackathon
def budget_check(expense_price) : 
    global remaining_budget
    if expense_price<=remaining_budget:
        remaining_budget-=expense_price
        return True
    else:
        return False
        

def add_expense():
    expense_name=input("\nEnter the name of the expense: ")
    expense_price=float(input("Enter the price ($) of {:s}: ".format(expense_name.lower())))
    if budget_check(expense_price)==True:
        expenses[expense_name.upper()]= expense_price
    else:
        overbudget_choice=input("\nThis expense is overbudget. Do you want to \
cancel this expense and add another expense (type cancel) or return to the main menu (type return)?\n").lower()
        if overbudget_choice=="cancel":
            add_expense()
    return

def remove_expense():
    global expenses
    global remaining_budget
    expense_list=[]
    for expense in expenses:
        expense_list.append([expense,expenses[expense]])
    print("\nCURRENT EXPENSES:")
    for item_num in range (1,len(expense_list)+1):
        print("#{:<5}|{:15s}|${:.2f}".format(item_num,expense_list[item_num-1][0],expense_list[item_num-1][1]))
    while True:
        decision=input("\nType which item number you would like to remove (type cancel to exit): ")
        if decision.lower()=="cancel":
            return
        elif decision.isnumeric()==False or int(decision)>len(expense_list) or int(decision)<1:
            print("\nError: Decision does not exist. Try Again.")
        else:
            remaining_budget+=expense_list[int(decision)-1][1]
            expenses.pop(expense_list[int(decision)-1][0])
            expense_list.pop(int(decision)-1)
            break
    print("\n*Your changes have been made*\n\nUPDATED EXPENSES:")
    for item_num in range (1,len(expense_list)+1):
        print("#{:<5}|{:15s}|${:.2f}".format(item_num,expense_list[item_num-1][0],expense_list[item_num-1][1]))
    

def list_settings():
    list_option=input("\nType A to display the budget \nType B to change the price of an expense\
\nType C to change the name of an expense\nType D to return to main menu\n").upper()
    if list_option=="A":
        print("\n")
        expense_number_counter=1
        for expense,expense_price in expenses.items():
            print("#{:<5}|{:15s}|${:.2f}".format(expense_number_counter,expense,expense_price))
            expense_number_counter+=1
        list_settings()
    elif list_option=="B":
        change_expense_price()
    elif list_option=="C":
        change_expense_name()
    elif list_option=="D":
        return
    else:
        print("ERROR enter a valid input")
        list_settings()
        
def change_expense_price():
        global remaining_budget
        updated_expense=input("\nEnter name of expense or enter ""return"" to return to list settings: ").upper()
        if updated_expense=="RETURN":
            list_settings()
            return
        if updated_expense in expenses.keys():
            updated_expense_price= float(input("Enter the new price ($) of the {:s}: ".format(updated_expense)))
            if expenses[updated_expense]< updated_expense_price:
                if budget_check(updated_expense_price-expenses[updated_expense])==True:
                    expenses[updated_expense]= updated_expense_price
                else:
                    while True: 
                        overbudget_choice=input("\nThis change in expense is overbudget. Do you want to \
cancel this change and perform another change (type cancel) or return to the list settings (type return)?\n").lower()
                        if overbudget_choice=="cancel":
                            change_expense_price()
                        elif overbudget_choice=="return":
                            list_settings()
                        else:
                            print("ERROR enter a valid input")
                        
            else:
                remaining_budget+=(expenses[updated_expense]-updated_expense_price)
                expenses[updated_expense]= updated_expense_price
            print("\n{:<s}'s price is now ${:,.2f}".format(updated_expense,updated_expense_price))
            list_settings()
            return
        else:
            print("ERROR this expense is NOT in your list. Check your spelling or add it to your list first!")
            change_expense_price()
            return
        
def change_expense_name():
    old_name=input("\nEnter name of expense or enter ""return"" to return to list settings: ").upper()
    if old_name=="RETURN":
        list_settings()
        return
    if old_name in expenses.keys():
        temp_value=expenses[old_name]
        expenses.pop(old_name)
        updated_name=input("Enter the new name for this expense: ").upper()
        expenses[updated_name]=temp_value
        print("\nThe name has been updated")
        list_settings()
        return
    else:
        print("ERROR This expense does not exist in your list. Check your spelling or add it to your list first!")
        change_expense_name()
        return
        

def list_budget():
    global remaining_budget
    global starting_budget
    print("\nYou started with ${:,.2f} and you spent ${:,.2f}. Now you have ${:,.2f}$ left".format\
          (starting_budget,starting_budget-remaining_budget,remaining_budget))
    change_starting_budget=input("Would you like to change your starting budget (Enter yes or no)?\n").lower()
    if change_starting_budget=="yes":
        new_starting_budget=float(input("Enter your NEW starting budget:\n"))
        if new_starting_budget<(starting_budget-remaining_budget):
            print("ERROR insufficient funds to accomodate new starting budget, reduce prices of expenses or remove expenses!")
            return
        else:
            if new_starting_budget>starting_budget:
                remaining_budget+=abs(starting_budget-new_starting_budget)
            else:
                remaining_budget-=abs(starting_budget-new_starting_budget)
            starting_budget=new_starting_budget                      
            print("The new starting budget is now ${:,.2f}".format(starting_budget))
            return
    elif change_starting_budget=="no":
        return
    else:
        print("ERROR enter a valid input")
        list_budget()
    

def end():
    print("\n")
    for expense,expense_price in expenses.items():
        print("{:<s}: ${:,.2f}".format(expense,expense_price))
    print("\nYou started with ${:,.2f}$ and you spent ${:,.2f}. Now you have ${:,.2f} left".format\
          (starting_budget,starting_budget-remaining_budget,remaining_budget))
    return
    
starting_budget=float(input("Enter your starting budget ($): "))
remaining_budget=float(starting_budget)
expenses={}
option=""

while option!="E":
    option=input("\nType A to add an expense to the budget \nType B to remove an expense from the budget\
\nType C to open list settings\nType D to check your starting and remaining budget or change starting budget \nType E to end and show budget and remaining budget:\n").upper()
    if option=="A":
        add_expense()
    elif option=="B":
        remove_expense()
    elif option=="C":
        list_settings()
    elif option=="D":
        list_budget()
    elif option=="E":
        end()
    else:
        print("ERROR Enter a valid input")
          
    
