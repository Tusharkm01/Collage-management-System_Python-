#Collage-management-System_Python-
#csv file have to modified 


    import csv
    import os
    class CollegeManagementSystem:
    def __init__(self, file_path='colleges.csv'):
        self.file_path = file_path
        self.colleges = self.load_colleges()

    def load_colleges(self):
        if not os.path.exists(self.file_path):
            return []  # Return an empty list if the file does not exist

        with open(self.file_path, mode='r', newline='') as file:
            reader = csv.DictReader(file)
            return [row for row in reader]

    def save_colleges(self):
        with open(self.file_path, mode='w', newline='') as file:
            fieldnames = self.colleges[0].keys() if self.colleges else []
            writer = csv.DictWriter(file, fieldnames=fieldnames)
            writer.writeheader()
            writer.writerows(self.colleges)

    def display_menu(self):
        print("\nCollege Management System Menu:")
        print("1. View all colleges")
        print("2. Filter colleges by location")
        print("3. Get best college (by highest offer)")
        print("4. View colleges supporting CSE")
        print("5. Add new college")
        print("6. Remove colleges with low package offers (<3L)")
        print("7. Apply custom filters")
        print("8. Exit")

    def apply_filters(self):
        print("\nAvailable filters:")
        filters = ['College ID', 'College Name', 'Tier', 'Fees', 'Highest Rank', 'Location', 'Highest Offer']
        for i, filter_name in enumerate(filters, 1):
            print(f"{i}. {filter_name}")

        filter_choices = input("Enter filter numbers to apply (comma separated): ").split(',')
        filtered_colleges = self.colleges

        for choice in filter_choices:
            try:
                col_name = filters[int(choice) - 1]
                filter_value = input(f"Enter value for {col_name}: ")
                filtered_colleges = [college for college in filtered_colleges if college[col_name].lower() == filter_value.lower()]
            except (ValueError, IndexError):
                print("Invalid filter selection")
                return

        self.display_colleges(filtered_colleges)

    def display_colleges(self, colleges):
        if not colleges:
            print("No colleges found.")
            return
        print("\nColleges:")
        for college in colleges:
            print(college)

    def view_cse_colleges(self):
        # In a real system, you'd check actual course offerings
        print("\nAll colleges (assuming all support CSE):")
        self.display_colleges(self.colleges)

    def run(self):
        while True:
            self.display_menu()
            choice = input("Enter your choice (1-8): ")

            if choice == '1':
                self.display_colleges(self.colleges)
            elif choice == '2':
                location = input("Enter location to filter by: ")
                filtered = [college for college in self.colleges if college['Location'].lower() == location.lower()]
                self.display_colleges(filtered)
            elif choice == '3':
                best = max(self.colleges, key=lambda x: float(x['Highest Offer']))
                print("\nBest College:")
                print(best)
            elif choice == '4':
                self.view_cse_colleges()
            elif choice == '5':
                self.add_college()
            elif choice == '6':
                self.colleges = [college for college in self.colleges if float(college['Highest Offer']) >= 300000]
                self.save_colleges()
                print("Low package colleges removed successfully")
            elif choice == '7':
                self.apply_filters()
            elif choice == '8':
                self.save_colleges()
                print("Goodbye!")
                break
            else:
                print("Invalid choice. Please try again.")

    def add_college(self):
        print("\nAdd New College:")
        new_college = {}
        for col in ['College ID', 'College Name', 'Tier', 'Fees', 'Highest Rank', 'Location', 'Highest Offer']:
            value = input(f"Enter {col}: ")
            new_college[col] = value
        
        self.colleges.append(new_college)
        self.save_colleges()
        print("College added successfully!")

if __name__ == "__main__":
    system = CollegeManagementSystem()
    print("\nWelcome to College Management System")
    print("------------------------------------")
    system.run()
    
