# importing module
from pymongo import MongoClient
try:
        # creation of MongoClient
        client=MongoClient()
        print("Connection Successfully!!")
except:
        print("Could Not Connect to MongoDB!!")

# Connect with the portnumber and host
client = MongoClient("mongodb://localhost:27017/")

# Access database
# Method 2 :mydatabase = client['myDB']
mydatabase = client.myDB

# Access collection of the database
# Method 2 : mycollection=mydatabase['user_Table']
mycollection = mydatabase.user_Table

def insert(id,name,age,city):
        # dictionary to be added in the database
        record={
                'ID': id,
                'Name': name,
                'Age': age,
                'city': city
                }
                
        # inserting the data in the database
        #rec =mycollection.insert_one(record)
        mycollection.insert_one(record)
        print("Data Insert Success")

def update(name, age, city,id):

        # update all the user data with given ID
        result = mycollection.update_many(
                {"ID":id}, # update filter
                { #update action
                        "$set":{
                                "Name": name,
                                "Age": age,
                                "city": city
                                },
                        "$currentDate":{"lastModified":True}
                        }
                )      
        print("Data updated with id",result)
        print("Total Record Updated",result.matched_count)

def showRec():
        # To find() all the entries inside collection name 'user_Table'
        cursor = mycollection.find()
        for record in cursor:
                print(record)

def delete(id):
        # deletes a document of user data with given ID
        deleteFilter = {'ID': id}

        result= mycollection.delete_many(deleteFilter)
        #Alternative you can use delete_one ::
mycollection.delete_one(deleteFilter)

        print("Data deleted with id",result)


while True:
                print("1.Insert Data")
                print("2.Update Data")
                print("3.Print Data")
                print("4.Delete Data")
                print("5.Exit")
                choice = int(input("Enter Your Choice : "))
                if choice == 1:
                        id = input("Enter The Id : ")
                        name = input("Enter Name : ")
                        age = input("Enter Age : ")
                        city = input("Enter City : ")
                        insert(id, name, age, city)
                        #,name, age, city)
                elif choice == 2:
                        id = input("Enter the ID to be updated: ")
                        name = input("Enter new Name : ")
                        age = input("Enter new Age : ")
                        city = input("Enter new City : ")
                        update(name, age, city,id)
                elif choice == 3:
                        showRec()
                elif choice == 4:
                        id = input("Enter The ID to Delete : ")
                        delete(id)
                elif choice == 5:
                        quit()
                else:
                        print("Invalid Selection . Please Try Again !")
