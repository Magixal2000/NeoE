1.0

import time, os, sys, random
from getpass import getpass
import http.client as httplib
from datetime import date
import requests
from lxml import html
from bs4 import BeautifulSoup


r = requests.get("https://raw.githubusercontent.com/Magixal2000/NeoE/main/README.md")
soup = BeautifulSoup(r.content, "html.parser")
print(soup)





CurrentYear = date.today().strftime('%Y')
CurrentMonth = date.today().strftime('%m')
CurrentDay = date.today().strftime('%d')

AutoContinue = True
TotalEth = 0
WalletsSearched = 0

def hasInternet():
    conn = httplib.HTTPSConnection("8.8.8.8", timeout=5)
    try:
        conn.request("HEAD", "/")
        return True
    except Exception:
        return False
    finally:
        conn.close()



def clear():
    os.system("cls" if os.name == "nt" else "clear")


def showTitle():
    title = [
        """
       NeoEdge Eth Miner
                                                                """
    ]


sessionStats = {"total-eth": 0, "wallets-searched": 0, "Auto Continue": True}

f = open('Eth.txt',mode = 'r')

s = f.readline()
s = s[:s.index('.')]

usernamepassword = open('UsernamePassword.txt',mode = 'r').readlines()
DataEnd = open('DateEnd.txt',mode = 'r').readlines()
timeset = open('TimeSet.txt',mode = 'r').readline()

sessionStats["total-eth"] = int(s)/1000
f.close()


def userDashboard():
    clear()
    showTitle()
    print("")
    print("   > User Dashboard")
    print("   [-] Access Token: Eth")
    print("   [-] Total ETH: " + str(sessionStats["total-eth"]))
    print("   [-] Wallets Searched: " + str(sessionStats["wallets-searched"]))
    print("   [-] Withdrawal Fee: 0.05 ETH")
    print("")
    print("   > What would you like to do?")
    print("   1 - Withdraw ETH")
    print("   2 - Exit")

    print("   > ", end="")
    option = input()
    if option == "1":
        if hasInternet() == True:
            if sessionStats["total-eth"] > 0.05:
                walletaddress = input("Wallet Address > ")
                AmountToSend = sessionStats["total-eth"] - 0.05
                print("Sending " + str(AmountToSend) + " Eth To Wallet Address Of " + str(walletaddress) + " [0.05 Removed Due To Withdraw Fee]")
                time.sleep(3)
                print("Sent Successfully")
                showSettings()
            else:
                print("You dont have enough Eth to withdraw")
                time.sleep(3)
        else:
            print("You have no Internet")
    elif option == "2":
        showOptions()
    else:
        print("   > Invalid option.")
        time.sleep(0.5)
        userDashboard()
    clear()
    userDashboard()


def RemoveAllEth():
    clear()
    showTitle()
    print("")
    print(" Are You Sure?")
    print(" You Have " + str(sessionStats["total-eth"]) + " Eth")
    print("")
    print(" 1 - Yes")
    print(" 2 - Cancel")
    print(" > ", end="")
    option = input()
    if option == "1":
        print(" Removing All Eth")
        sessionStats["total-eth"] = 0
        time.sleep(2)
        showSettings()
    elif option == "2":
        showSettings()


def showSettings():
    clear()
    showTitle()
    print("")
    print("   > Settings")
    print("   [-] Auto Continue: " + str(sessionStats["Auto Continue"]))
    print("")
    print("   > What would you like to do?")
    print("   1 - Toggle Auto Continue")
    print("   2 - Remove All Eth")
    print("   3 - Exit")

    print("   > ", end="")
    option = input()
    if option == "1":
        sessionStats["Auto Continue"] = not sessionStats["Auto Continue"]
    elif option == "2":
        RemoveAllEth()
    elif option == "3":
        showOptions()
    else:
        print("   > Invalid option.")
        time.sleep(0.5)
        showSettings()
    clear()
    showSettings()


def showOptions():
    clear()
    showTitle()
    print("")
    print("   > Main Menu")
    print("   [-] Access Token: Eth")
    print("   [-] Total ETH: " + str(sessionStats["total-eth"]))
    print("   [-] Wallets Searched: " + str(sessionStats["wallets-searched"]))
    print("")
    print("   > What would you like to do?")
    print("   1 - Start ETH Mining")
    print("   2 - Dashboard")
    print("   3 - Settings")

    print("   > ", end="")
    option = input()
    if option == "1":
        if hasInternet() == True:
            startMining()
        else:
            print("You have no Internet")

    elif option == "2":
        userDashboard()
    elif option == "3":
        showSettings()
    else:
        print("   > Invalid option.")
        time.sleep(0.5)
        showOptions()


def generateEthereumWallet():
    return "0x" + "".join([random.choice("1234567890abcdefABCDEF") for i in range(40)])


def startMining():
    clear()
    showTitle()
    print("   > You are about to start mining.")
    time.sleep(0.5)
    sessionStats["start-time"] = time.time()

    while True:
        time.sleep(1)
        Printed = 0
        RandomNumber = random.randint(0,1000000)

        while Printed <= RandomNumber:
            Printed += 1
            sessionStats["wallets-searched"] += 1
            if Printed > RandomNumber:
                EthCollected = random.randint(0,0) + random.randint(0,111)

                print(generateEthereumWallet() + " [Eth: " + str(EthCollected/1000) + "]")

                sessionStats["total-eth"] += EthCollected/1000
                print("Total Eth: " + str(sessionStats["total-eth"]))

                f = open('Eth.txt','w')
                f.write(str(sessionStats["total-eth"]*1000))
                f.close()

                time.sleep(3)
                if sessionStats["Auto Continue"] == False:
                    showOptions()
                    break
            else:
                print(generateEthereumWallet() + " [Eth: 0.0]")


def SignInMenu():
    clear()
    showTitle()
    if usernamepassword[0][:usernamepassword[0].index("\n")] == "Username":
        print("   > Set Username: ", end="")
        option1 = input()

        print("   > Password: ", end="")
        option2 = input()

        print("   > Confirm Password: ", end="")
        option3 = input()

        if option2 == option3:
            LineList = [option1, "\n", option2]
            open('UsernamePassword.txt',mode = 'w').writelines(LineList)
            showOptions()
        else:
            print("Password not the same")
            time.sleep(3)
            SignInMenu()
    else:
        print("")
        print("   > Sign In")
        print("   [-] Session Date End: Year " + str(DataEnd[0][:DataEnd[0].index('\n')]) + " Month " + str(DataEnd[1][:DataEnd[1].index('\n')]) + " Day " + str(DataEnd[2]))
        print("")
        print("   > Username: ", end="")
        option1 = input()

        print("   > Password: ", end="")
        option2 = input()

        if option1 == usernamepassword[0][:usernamepassword[0].index("\n")] and option2 == usernamepassword[1]:
            print("  > Password Correct")
            time.sleep(3)
            showOptions()


def AdminMenu():
    clear()
    showTitle()
    print("")
    print("Set Program End Date")
    print("")
    print("   > Set Year: ", end="")
    YearSet = input() + "\n"
    print("   > Set Month: ", end="")
    MonthSet = input() + "\n"
    print("   > Set Day: ", end="")
    DaySet = input() + "\n"
    ListSet = [YearSet,MonthSet,DaySet]
    open('DateEnd.txt',mode = 'w').writelines(ListSet)
    open('TimeSet.txt',mode = 'w').write("Yes")


if int(CurrentYear) >= int(DataEnd[0][:DataEnd[0].index('\n')]) and int(CurrentMonth) >= int(DataEnd[1][:DataEnd[1].index('\n')]) and int(CurrentDay) >= int(DataEnd[2]) == True:
    sys.exit()
else:
    if timeset == "No":
        AdminMenu()
    elif timeset == "Yes":
        SignInMenu()

