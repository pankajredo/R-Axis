import tkinter as tk
from tkinter import Frame, Label, Entry, Menu, Canvas, Button, Toplevel
# from PIL import Image, ImageTk
from tkinter import ttk, messagebox
from datetime import datetime
import pyodbc
import os
import threading
import subprocess
from tkinter import messagebox
# from sqlalchemy import create_engine
from tkinter import Tk, Label, Button, messagebox
import webbrowser
# import pandas as pd
# from sqlalchemy.engine import URL
# from fpdf import FPDF
# import matplotlib.pyplot as plt

# Function to create a new page
def create_page(title):
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    if title == 'Exploratory Vibration':
        create_exploratory_vibration_page()
    elif title == 'Variable Vibration':
        create_variable_vibration_page()
    elif title == 'Endurance Vibration Test':
        create_endurance_vibration_page()
    elif title == 'Impulse Test':
        create_impulse_test_page()
    elif title == 'Flexure Fatigue Test':
        create_flexure_fatigue_test_page()
    elif title == 'Rotary Flexure Test':
        create_rotary_fatigue_test_page()
    elif title == 'Thermal Cycle Test':
        create_thermal_cycle_test_page()
    elif title == 'Elevated Temperature Soak Test':
        create_elevated_tempature_soak_test_page()
    elif title == 'Hydro Test':
        create_hydro_test_page()
    elif title == 'Pneumatic Test':
        create_pneumatic_test_page()
    elif title == 'Bust Test':
        create_bust_test_page()
    else:        
        label = tk.Label(main_frame, text=title, font=("Helvetica", 16))
        label.pack(pady=25, expand=True)

# Function to create the home page
def create_home_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title for the home page with increased font size
    title = "Home Page"
    label = tk.Label(main_frame, text=title, font=("Helvetica", 25))  # Increased font size
    label.grid(row=0, column=0, columnspan=4, pady=5, sticky='nsew')
    
    buttons = [
        'Exploratory Vibration',
        'Variable Vibration', 
        "Endurance Vibration Test", 
        "Impulse Test",
        "Flexure Fatigue Test",
        "Rotary Flexure Test",
        "Thermal Cycle Test",
        "Elevated Temperature Soak Test",
        "Hydro Test",
        "Pneumatic Test",
        "Bust Test"
    ]
    num_columns = 4  # Number of columns in the grid
    row = 1
    col = 0

    for button_text in buttons:
        # Create a button with reduced height
        button = tk.Button(main_frame, text=button_text, bg="yellow", command=lambda bt=button_text:create_page(bt), font=("Helvetica", 15), padx=10, pady=2)
        
        # Configure button to fit within the grid cell
        button.grid(row=row, column=col, padx=5, pady=2, sticky='nsew')
        
        # Update row and column for the next button
        col += 1
        if col >= num_columns:
            col = 0
            row += 1

    # Adjust grid configuration for responsiveness
    for i in range(row + 1):
        main_frame.grid_rowconfigure(i, weight=1)
    for i in range(num_columns):
        main_frame.grid_columnconfigure(i, weight=1)



def logout():
    root.destroy()

def open_link():
    webbrowser.open_new("http://www.Redogroup.in")

def create_rounded_button(canvas, x, y, width, height, radius, text, color, command=None):
    points = [x+radius, y,
                x+width-radius, y,
                x+width, y,
                x+width, y+radius,
                x+width, y+height-radius,
                x+width, y+height,
                x+width-radius, y+height,
                x+radius, y+height,
                x, y+height,
                x, y+height-radius,
                x, y+radius,
                x, y]
    canvas.create_polygon(points, smooth=True, fill=color, outline=color)
    
    button = Button(canvas, text=text, command=command, font=('Arial', 12, 'bold'), bg=color, fg='white', activebackground=color, activeforeground='white', borderwidth=0)
    canvas.create_window(x + width // 2, y + height // 2, window=button)


# Function to create the Exploratory Vibration page
def create_exploratory_vibration_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Exploratory Vibration Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="both", expand=True)
    
    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Exploratory Vibration Test Summary")

        form_label = Label(popup, text="Summary of Exploratory Vibration Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack() 

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
                    
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        support_distance_label = Label(popup, text="SUPPORT DISTANCE", font=("Arial", 12))
        support_distance_label.pack()

        support_distance_entry = Entry(popup, width=25)
        support_distance_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), support_distance_entry.get()))

        submit_button.pack()

        # Center the popup window on the screen
        popup.update_idletasks() 
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))
    
    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_exploratery_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\exploratery_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)

    # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO", "SIZE", "SUPPORT DISTANCE"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, support_distance FROM dbo.exploratary_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Pressure(Bar)", "Set High(Bar)", "Set Low(Bar)", "Set cycle_time (Min)", "Motor Frequency", "Actual Min", "Table Vibration", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_Pressure, Set_High, Set_Low, Set_cycle_time, Motor_freq, Actual_Min, table_Vibration, Cycle
            FROM tbl_exploratary_vibration
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Pressure\n(Bar)", "Set High\n(Bar)", "Set Low\n(Bar)",
        "Set Cycle Time\n(Min)", "Motor Frequency", "Actual Min", "Table Vibration\n(G-force)", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_Pressure, 
                Set_High, 
                Set_Low, 
                Set_cycle_time, 
                Motor_freq, 
                Actual_Min, 
                table_Vibration, 
                Cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM dbo.tbl_exploratary_vibration 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()    

    def insert_data(popup, test_no, sample_no, size, support_distance):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO exploratary_summary (test_no, sample_no, size, support_distance) 
                VALUES (?, ?, ?, ?)
            ''', (test_no, sample_no, size, support_distance))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM exploratary_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_exoloratery_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, support_distance FROM dbo.exploratary_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)



    def execute_exe_file(exe_path):
        try:
            subprocess.run([exe_path], check=True)
            return True
        except FileNotFoundError:
            print(f"File not found: {exe_path}")
            return False
        except subprocess.CalledProcessError as e:
            print(f"Failed to execute {exe_path}: {e}")
            return False
        except Exception as e:
            print(f"An unexpected error occurred while trying to execute {exe_path}: {e}")
            return False    
    
    def generate_report(popup):
        if execute_exe_file([r"C:\R-axis\dist\Report_file\Bust_report.bat"],check=True):
            print("Report generated successfully.")
        else:
            print("Failed to generate the report.")
        popup.destroy()
        create_home_page()
    
def create_variable_vibration_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Variable Vibration Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Exploratory Vibration Test Summary")

        form_label = Label(popup, text="Summary of Exploratory Vibration Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
            
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        support_distance_label = Label(popup, text="SUPPORT DISTANCE", font=("Arial", 12))
        support_distance_label.pack()

        support_distance_entry = Entry(popup, width=25)
        support_distance_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda: insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), support_distance_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))
    
    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_variable_vib_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\variable_vib_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
    
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)


        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO", "SIZE", "SUPPORT DISTANCE"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, support_distance FROM variable_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Pressure(Bar)", "Set High(Bar)", "Set Low(Bar)", "Set cycle_time (Min)", "Motor Frequency", "Actual Min", "Table Vibration", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_Pressure,
            Set_High,
            Set_Low,
            Set_cycle_time,
            Motor_freq,
            Actual_Min,
            table_Vibration,
            Cycle
            FROM tbl_Variable_vibration
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Pressure\n(Bar)", "Set High\n(Bar)", "Set Low\n(Bar)",
        "Set Cycle Time\n(Min)", "Motor Frequency", "Actual Min", "Table Vibration\n(G-force)", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_Pressure, 
                Set_High, 
                Set_Low, 
                Set_cycle_time, 
                Motor_freq, 
                Actual_Min, 
                table_Vibration, 
                Cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM dbo.tbl_Variable_vibration 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no, size, support_distance):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO variable_summary (test_no, sample_no, size, support_distance) 
                VALUES (?, ?, ?, ?)
            ''', (test_no, sample_no, size, support_distance))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM variable_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Var_vib_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, support_distance FROM dbo.variable_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

    def execute_exe_file(exe_path):
        try:
            subprocess.run([exe_path], check=True)
            return True
        except FileNotFoundError:
            print(f"File not found: {exe_path}")
            return False
        except subprocess.CalledProcessError as e:
            print(f"Failed to execute {exe_path}: {e}")
            return False
        except Exception as e:
            print(f"An unexpected error occurred while trying to execute {exe_path}: {e}")
            return False

def create_endurance_vibration_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Endurance Vibration Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    
    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Exploratory Vibration Test Summary")

        form_label = Label(popup, text="Summary of Exploratory Vibration Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
            
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        support_distance_label = Label(popup, text="SUPPORT DISTANCE", font=("Arial", 12))
        support_distance_label.pack()

        support_distance_entry = Entry(popup, width=25)
        support_distance_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), support_distance_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))
        
    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Endurance_vib_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Endurance_vib_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)


    # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO", "SIZE", "SUPPORT DISTANCE"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, support_distance FROM endurance_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Pressure(Bar)", "Set High(Bar)", "Set Low(Bar)", "Set cycle_time (Min)", "Motor Frequency", "Actual Min", "Table Vibration", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_Pressure, Set_High, Set_Low, Set_cycle_time, Motor_freq, Actual_Min, table_Vibration, Cycle
            FROM tbl_endurance_vibration
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Pressure\n(Bar)", "Set High\n(Bar)", "Set Low\n(Bar)",
        "Set Cycle Time\n(Min)", "Motor Frequency", "Actual Min", "Table Vibration\n(G-force)", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_Pressure, 
                Set_High, 
                Set_Low, 
                Set_cycle_time, 
                Motor_freq, 
                Actual_Min, 
                table_Vibration, 
                Cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_endurance_vibration 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no, size, support_distance):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO endurance_summary (test_no, sample_no, size, support_distance) 
                VALUES (?, ?, ?, ?)
            ''', (test_no, sample_no, size, support_distance))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM endurance_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Endurance_vib_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, support_distance FROM dbo.endurance_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()


    def show_generate_report_popup():
        popup = Toplevel(root)
        popup.title("Generate Report")

        message_label = Label(popup, text="Do you want to Generate Report?", font=("Arial", 14))
        message_label.pack(pady=10)

        button_frame = Frame(popup)
        button_frame.pack(pady=10)

        yes_button = Button(button_frame, text="Yes", command=lambda: generate_report(popup))
        yes_button.pack(side="left", padx=10)

        no_button = Button(button_frame, text="No", command=popup.destroy)
        no_button.pack(side="left", padx=10)

        # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))
        
        # canvas.create_window(x + width // 2, y + height // 2, window=Button)
    
    def generate_report(popup):
        if execute_exe_file('C:\\R-axis\\dist\\exploratory_test_report.exe'):
            print("Report generated successfully.")
        else:
            print("Failed to generate the report.")
        popup.destroy()

    def execute_exe_file(exe_path):
        try:
            subprocess.run([exe_path], check=True)
            return True
        except FileNotFoundError:
            print(f"File not found: {exe_path}")
            return False
        except subprocess.CalledProcessError as e:
            print(f"Failed to execute {exe_path}: {e}")
            return False
        except Exception as e:
            print(f"An unexpected error occurred while trying to execute {exe_path}: {e}")
            return False

def create_impulse_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Impulse Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Impulse Test Test Summary")

        form_label = Label(popup, text="Summary of Impulse Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
            
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        support_distance_label = Label(popup, text="FLUID", font=("Arial", 12))
        support_distance_label.pack()

        support_distance_entry = Entry(popup, width=25)
        support_distance_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), support_distance_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))
    
    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Impulse_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Impulse_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)


        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO", "SIZE", "Fluid"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, fluid FROM Impulse_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Pressure(Bar)", "Actual Cycle(Bar)", "Set Cycle", "On Time (Min)", "Off Time (Min)", "Actual Min", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_Pressure, Actual_cycle, Set_cycle, on_time, off_time, actual_min, Cycle
            FROM tbl_new_impulse
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Pressure\n(Bar)", "Actual Cycle\n(Hz)", "Set Cycle\n(Hz)",
        "On Time\n(Min)", "Off Time\n(Min)", "Actual Min", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_Pressure, 
                Actual_cycle, 
                Set_cycle, 
                on_time, 
                off_time, 
                actual_min, 
                Cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_impulse 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no, size, fluid):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO Impulse_summary (test_no, sample_no, size, fluid) 
                VALUES (?, ?, ?, ?)
            ''', (test_no, sample_no, size, fluid))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM Impulse_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Impulse_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, fluid FROM dbo.Impulse_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

def create_flexure_fatigue_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Flexure Fatigue Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.38 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.15 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.17 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.17 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=5, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.04 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=5, pady=5, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Flexure Fatigue Test Summary")

        form_label = Label(popup, text="Summary of Flexure Fatigue Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
            
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        Atmospheric_Temp_label = Label(popup, text="Atmospheric Temp", font=("Arial", 12))
        Atmospheric_Temp_label.pack()

        Atmospheric_Temp_entry = Entry(popup, width=25)
        Atmospheric_Temp_entry.pack()
        
        Deflection_Strain_label = Label(popup, text="Deflection Strain", font=("Arial", 12))
        Deflection_Strain_label.pack()

        Deflection_Strain_entry = Entry(popup, width=25)
        Deflection_Strain_entry.pack()
        
        Bidirectional_Flexure_label = Label(popup, text="Bidirectional Flexure", font=("Arial", 12))
        Bidirectional_Flexure_label.pack()

        Bidirectional_Flexure_entry = Entry(popup, width=25)
        Bidirectional_Flexure_entry.pack()
        
        Calculated_Axial_Stress_label = Label(popup, text="Calculated Axial Stress", font=("Arial", 12))
        Calculated_Axial_Stress_label.pack()

        Calculated_Axial_Stress_entry = Entry(popup, width=25)
        Calculated_Axial_Stress_entry.pack()
        
        Stress_Values_label = Label(popup, text="Stress Values", font=("Arial", 12))
        Stress_Values_label.pack()

        Stress_Values_entry = Entry(popup, width=25)
        Stress_Values_entry.pack()
        
        Room_Temp_label = Label(popup, text="Room Temp", font=("Arial", 12))
        Room_Temp_label.pack()

        Room_Temp_entry = Entry(popup, width=25)
        Room_Temp_entry.pack()
        
        Material_label = Label(popup, text="Material", font=("Arial", 12))
        Material_label.pack()

        Material_entry = Entry(popup, width=25)
        Material_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), Atmospheric_Temp_entry.get(), Deflection_Strain_entry.get(), Bidirectional_Flexure_entry.get(), Calculated_Axial_Stress_entry.get(), Stress_Values_entry.get(), Room_Temp_entry.get(), Material_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=5, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Flexure_Fatigue_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Flexure_fat_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()

        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)

        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.35 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=5, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["Test No", "Sample No", "Size", "Atmospheric Temp", "Deflection Strain","Bidirectional Flexure","Calculated Axial Stress","Stress Values", "Room Temp", "Material"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, atmospheric_temp, deflection_strain, bidirectional_flexture,calculated_axial_stress, stress_values, room_temp, material FROM flexure_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Pressure(Bar)", "Set High(Bar)", "Set Low(Bar)", "Set Cycle (Hz)","Cycle On Time(Min)","Cycle Off Time(Min)","Actual Cycle", "Cycle Stop High", "Cycle Stop Low", "Stress Value", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_Pressure, Set_High, Set_Low, Set_cycle, cycle_on_time, cycle_off_time, Actual_cycle,cycle_stop_high,cycle_stop_low,stress_value, Cycle
            FROM tbl_new_flexure_fatigue
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Pressure\n(Bar)", "Set High\n(Bar)", "Set Low\n(Bar)",
        "Set Cycle\n(Hz)", "Cycle On Time\n(Min)", "Cycle Off Time\n(Min)", "Cycle Stop High\n(Hz)","Cycle Stop Low\n(Hz)","Stress Values" "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_Pressure, 
                Set_High, 
                Set_Low, 
                Set_cycle,
                cycle_on_time,
                cycle_off_time, 
                Actual_cycle, 
                cycle_stop_high, 
                cycle_stop_low,
                stress_value,
                Cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_flexure_fatigue
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no, size, atmospheric_temp, deflection_strain, bidirectional_flexture, calculated_axial_stress, stress_values, room_temp, material):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO flexure_summary (test_no, sample_no, size, atmospheric_temp, deflection_strain, bidirectional_flexture, calculated_axial_stress, stress_values, room_temp, material) 
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            ''', (test_no, sample_no, size, atmospheric_temp, deflection_strain, bidirectional_flexture, calculated_axial_stress, stress_values, room_temp, material))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM flexure_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Flexure_Fatigue_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, atmospheric_temp, deflection_strain, bidirectional_flexture, calculated_axial_stress, stress_values, room_temp, material FROM dbo.flexure_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None, None, None, None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

    def execute_exe_file(exe_path):
        try:
            subprocess.run([exe_path], check=True)
            return True
        except FileNotFoundError:
            print(f"File not found: {exe_path}")
            return False
        except subprocess.CalledProcessError as e:
            print(f"Failed to execute {exe_path}: {e}")
            return False
        except Exception as e:
            print(f"An unexpected error occurred while trying to execute {exe_path}: {e}")
            return False

def create_rotary_fatigue_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=40)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Rotary Flexure Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.45 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=5, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.25 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=5, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Rotary Flexure Test Summary")

        form_label = Label(popup, text="Summary of Rotary Flexure Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
            
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        fluid_label = Label(popup, text="FLUID", font=("Arial", 12))
        fluid_label.pack()

        fluid_entry = Entry(popup, width=25)
        fluid_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda: insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), fluid_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Rotary_fatigue_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Rotary_fatigue_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)


        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO", "SIZE", "FLUID"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, fluid FROM rotary_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Pressure(Bar)", "Set High(Bar)", "Set Low(Bar)", "Set Cycle (Hz)", "RPM", "Actual Cycle", "Cycle Stop High", "Cycle Stop Low", "Stress Value", "Cycle"]

            if self.total_rows > len(labels):
                raise ValueError(f"Data has {self.total_rows} rows, but only {len(labels)} labels provided.")

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i + 1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i + 1, column=1, sticky='nsew')

                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_Pressure, Set_High, Set_Low, Set_cycle, rpm, Actual_cycle, cycle_stop_high, cycle_stop_low, stress_value, cycle
            FROM tbl_new_rotary_flexure
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Pressure\n(Bar)", "Set High\n(Bar)", "Set Low\n(Bar)",
        "Set Cycle\n(Hz)", "RPM", "Actual Cycle", "Cycle Stop High\n(Hz)", "Cycle Stop Low\n(Hz)", "Stress Value", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_Pressure, 
                Set_High, 
                Set_Low, 
                Set_cycle, 
                rpm, 
                Actual_cycle, 
                cycle_stop_high, 
                cycle_stop_low, 
                stress_value, 
                cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_rotary_flexure 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no, size, fluid):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO rotary_summary (test_no, sample_no, size, fluid) 
                VALUES (?, ?, ?, ?)
            ''', (test_no, sample_no, size, fluid))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM rotary_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Rotary_fatigue_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, fluid FROM dbo.rotary_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

    def execute_exe_file(exe_path):
        try:
            subprocess.run([exe_path], check=True)
            return True
        except FileNotFoundError:
            print(f"File not found: {exe_path}")
            return False
        except subprocess.CalledProcessError as e:
            print(f"Failed to execute {exe_path}: {e}")
            return False
        except Exception as e:
            print(f"An unexpected error occurred while trying to execute {exe_path}: {e}")
            return False

def create_thermal_cycle_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Thermal Cycle Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    
    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Thermal Test Summary")

        form_label = Label(popup, text="Summary of Thermal Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
        

        submit_button = Button(popup, text="Submit", command=lambda: insert_data(popup, test_no_entry.get(),sample_no_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))
    
    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Thermal_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Thermal_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)


        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 2
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no FROM theraml_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Temp(°C)", "High Temp(°C)","Low Temp(°C)", "Hold Time(Min)","Actual Min", "Stop Temp High (°C)","Stop Temp Low (°C)", "Set Pressure", "Actual Pressure", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_temp, High_temp, Low_temp, hold_time, actual_min, stop_temp_high, stop_temp_low,set_pressure, Actual_pressure, cycle
            FROM tbl_new_thermal
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Temp\n(°C)", "High Temp\n(°C)", "Low Temp\n(°C)", "Hold Time\n(Min)","Actual Min", "Stop Temp High\n(°C)","Stop Temp Low\n(°C)", "Set Pressure", "Actual Pressure", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_temp,
                High_temp,
                Low_temp,
                hold_time,
                actual_min,
                stop_temp_high,
                stop_temp_low,
                set_pressure,
                Actual_pressure, 
                cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_thermal
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO theraml_summary (test_no, sample_no) 
                VALUES (?, ?)
            ''', (test_no, sample_no))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM theraml_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Thermal_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no FROM dbo.theraml_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

def create_elevated_tempature_soak_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Elevated Tempature Soak Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.47 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Elevated Temperature Soak Test Summary")

        form_label = Label(popup, text="Summary of Elevated Temperature Soak Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()
            
        size_label = Label(popup, text="SIZE", font=("Arial", 12))
        size_label.pack()

        size_entry = Entry(popup, width=25)
        size_entry.pack()

        fluid_label = Label(popup, text="FLUID", font=("Arial", 12))
        fluid_label.pack()

        fluid_entry = Entry(popup, width=25)
        fluid_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get(), size_entry.get(), fluid_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Elevated_temp_soak_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Elevated_temp_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)

        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 4
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO", "SIZE","FLUID"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no, size, fluid FROM Elevated_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None, None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None, None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Temp(°C)", "Set High", "Set Low", "Set cycle_time (Min)", "Actual Cycle Time (Min)", "Set Pressure", "Actual Pressure", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_temp, set_high, set_low, set_cycle_time, actual_cycle_time, set_pressure, Actual_pressure, cycle
            FROM tbl_elev_temp_soak
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Temp(°C)", "Set High", "Set Low", "Set cycle_time (Min)", "Actual Cycle Time (Min)", "Set Pressure", "Actual Pressure", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS 
                SrNo,
                Actual_temp,
                set_high,
                set_low,
                set_cycle_time,
                actual_cycle_time,
                set_pressure,
                Actual_pressure,
                cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_elev_temp_soak 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no, size, fluid):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO Elevated_summary (test_no, sample_no, size, fluid) 
                VALUES (?, ?, ?, ?)
            ''', (test_no, sample_no, size, fluid))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM Elevated_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Elevated_temp_soak_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no, size, fluid FROM dbo.Elevated_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None, None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

def create_hydro_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Hydro Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.43 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.48 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.25 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=5, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.25 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=5, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=8, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Hydro Test Summary")

        form_label = Label(popup, text="Summary of Hydro Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda: insert_data(popup, test_no_entry.get(),sample_no_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Hydro_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Hydro_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)

        # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 2
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no FROM hydro_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Test Pressure(Bar)", "Set cycle_time (Min)", "Actual Cycle Time(min)", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_test_Pressure, set_cycle_time, actual_cycle_time, cycle
            FROM tbl_new_hydro
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Test Pressure\n(Bar)", "Set Cycle Time\n(min)", "Actual Cycle Time\n(min)", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_test_Pressure,
                set_cycle_time,
                actual_cycle_time,
                cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_hydro 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO hydro_summary (test_no, sample_no) 
                VALUES (?, ?)
            ''', (test_no, sample_no))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM hydro_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Hydro_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no FROM dbo.hydro_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

def create_pneumatic_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Pneumatic Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.43 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Pneumatic Test Summary")

        form_label = Label(popup, text="Summary of Pneumatic Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_Pneumatic_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Pneumatic_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)

    # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 2
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no FROM pneumatic_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Test Pressure(Bar)", "Set Cycle Time(min)", "Actual Cycle Time(min)", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_test_Pressure, set_cycle_time, Actual_cycle_time, cycle
            FROM tbl_new_pneumatic
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Test Pressure\n(Bar)", "Set Cycle Time\n(min)", "Actual Cycle Time\n(Min)", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_test_Pressure,
                set_cycle_time,
                Actual_cycle_time,
                cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_pneumatic 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO pneumatic_summary (test_no, sample_no) 
                VALUES (?, ?)
            ''', (test_no, sample_no))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM pneumatic_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_Pneumatic_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no FROM dbo.pneumatic_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

def create_bust_test_page():
    # Clear existing widgets
    for widget in main_frame.winfo_children():
        widget.destroy()
    
    # Title frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Bust Test', font=('Arial', 20, 'bold'), padx=10)
    title.pack(side='left')

    # Main container frame for the upper part (60% height)
    upper_frame = tk.Frame(main_frame, bg="white", height=int(0.6 * root.winfo_height()))
    upper_frame.pack(side="top", fill="both", expand=True)

    # Graph frame (60% width of the upper part)
    graph_frame = tk.Frame(upper_frame, bg='black', bd=3, relief="solid", width=int(0.6 * root.winfo_width()), height=int(0.43 * root.winfo_height()))
    graph_frame.pack(side="left", padx=10, pady=5, expand=True, fill="both")

    # Right container frame for summary and live data (40% width of the upper part)
    right_container_frame = tk.Frame(upper_frame, bg="white", width=int(0.4 * root.winfo_width()), height=int(0.6 * root.winfo_height()))
    right_container_frame.pack(side="left", fill="both", expand=True)

    # Summary data frame (occupies top half of right container frame)
    summary_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    summary_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    # Live data frame (occupies bottom half of right container frame)
    live_data_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.3 * root.winfo_height()))
    live_data_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)

    button_canvas_frame = tk.Frame(right_container_frame, borderwidth=2, relief="solid", height=int(0.05 * root.winfo_height()))
    button_canvas_frame.pack(side="bottom", fill="both", padx=10, pady=10, expand=True)

    button_canvas = Canvas(button_canvas_frame, bg="white")
    button_canvas.pack(fill="x", expand=True)

    def open_summary_popup():
        popup = Toplevel(root)
        popup.title("Bust Test Summary")

        form_label = Label(popup, text="Summary of Bust Test Cycle", font=("Arial", 16),background="blue", foreground="white", padx=10, pady=5)
        form_label.pack()

        test_no_label = Label(popup, text="TEST NO", font=("Arial", 12))
        test_no_label.pack()

        test_no_entry = Entry(popup, width=25)
        test_no_entry.pack()

        sample_no_label = Label(popup, text="SAMPLE NO", font=("Arial", 12))
        sample_no_label.pack()

        sample_no_entry = Entry(popup, width=25)
        sample_no_entry.pack()

        submit_button = Button(popup, text="Submit", command=lambda:insert_data(popup, test_no_entry.get(),sample_no_entry.get()))

        submit_button.pack()

    # Center the popup window on the screen
        popup.update_idletasks()
        width = popup.winfo_width()
        height = popup.winfo_height()
        x = (popup.winfo_screenwidth() // 2) - (width // 2)
        y = (popup.winfo_screenheight() // 2) - (height // 2)
        popup.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    start_button = tk.Button(button_canvas, text="Start Cycle", bg="#1C4587", fg="white", command=open_summary_popup)
    start_button.pack(padx=10, side='left', pady=10, expand=True)
    # Create the rounded buttons on the button canvas
    # create_rounded_button(button_canvas, 180, 10, 120, 40, 20, "Start Cycle", "#1C4587", command=open_summary_popup)
    
    def stop_cycle():
        try:
            # Run the batch file to perform any necessary cleanup or termination
            subprocess.run([r"C:\R-axis\dist\Start_stop_file\stop_bursting_cycle.bat"], check=True)
            # Show success message and ask if the user wants to generate the last cycle report
            root = tk.Tk()
            root.withdraw()  # Hide the root window

            result = messagebox.askyesno("Cycle Stopped Successfully", "Do you want to generate the last cycle report?")
            
            if result:
                # User clicked 'Yes'
                subprocess.run([r"C:\R-axis\dist\Start_stop_file\Burst_report.bat"], check=True)
                create_home_page()
            else:
                # User clicked 'No'
                create_home_page()
        except subprocess.CalledProcessError as e:
            # Handle error if the batch file fails
            messagebox.showerror("Error", f"Failed to stop the executable. Error: {e}")

    def stop_button_handler():
        threading.Thread(target=stop_cycle).start()
            
    stop_button = tk.Button(button_canvas, text="Stop Cycle", bg="#1C4587", fg="white", command=stop_button_handler)
    stop_button.pack(padx=10, pady=10,side="left", expand=True)
    # create_rounded_button(button_canvas, 380, 10, 160, 40, 20, "Stop Cycle", "#1C4587", command=stop_button_handler)

    # Data table frame (40% height of the window)
    data_table_frame = tk.Frame(main_frame, borderwidth=2,bg='pink', relief="solid", height=int(0.4 * root.winfo_height()))
    data_table_frame.pack(side="top", fill="both", padx=10, pady=10, expand=True)
    
    class Table:
        def __init__(self, root):
            self.entries = []

            thead_label = Label(root, text="Summary Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1)
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = 2
            self.total_columns = 2

            labels = ["TEST NO", "SAMPLE NO"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)

    def fetch_last_updated_row():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            query = "SELECT TOP 1 test_no, sample_no FROM busting_summary ORDER BY insert_datetime DESC"

            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()

            return row if row else (None, None)
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return None, None

    def update_summary_table():
        fetched_data = fetch_last_updated_row()
        if fetched_data:
            for entry, value in zip(table1.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))
        else:
            for entry in table1.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table1 = Table(summary_data_frame)
    update_summary_table()

    class Table2:
        def __init__(self, root, data):
            self.entries = []
            thead_label = Label(root, text="Live Data Value", font=('Arial', 16, 'bold'), bg='#1C4587', fg='white', relief='solid', borderwidth=1, justify='center')
            thead_label.grid(row=0, column=0, columnspan=2, sticky='nsew')

            self.total_rows = len(data)
            self.total_columns = 2

            labels = ["Actual Test Pressure(Bar)", "Set Cycle Time(min)", "Actual Cycle Time(min)", "Cycle"]

            for i in range(self.total_rows):
                label = Label(root, text=labels[i], font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, anchor='w')
                label.grid(row=i+1, column=0, sticky='nsew')

                entry = Entry(root, width=20, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                entry.grid(row=i+1, column=1, sticky='nsew')
                
                # Check if data[i] is a valid string or convertible to string
                entry.insert(tk.END, str(data[i]))  # Ensure value is converted to string
                self.entries.append(entry)

            for col in range(self.total_columns):
                root.grid_columnconfigure(col, weight=1)

            for row in range(self.total_rows + 1):
                root.grid_rowconfigure(row, weight=1)


    def fetch_live_updated_row():
        try:
            # Define server and database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Create connection string
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            # Establish connection
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()

            # SQL query to fetch the latest row
            query = """
            SELECT TOP 1 Actual_test_Pressure, Actual_cycle_time, Busting_pressure, cycle
            FROM tbl_new_bust
            ORDER BY insert_datetime DESC
            """

            # Execute the query
            cursor.execute(query)
            row = cursor.fetchone()

            # Close the connection
            conn.close()

            # Return the fetched row or a list of None if no row is found
            return row if row else [None] * 8

        except pyodbc.Error as e:
            # Print the SQL error
            print("Error fetching data from SQL table:", e)
            return [None] * 8
        
    def update_live_data_table():
        fetched_data = fetch_live_updated_row()
        if fetched_data:
            for entry, value in zip(table2.entries, fetched_data):
                entry.delete(0, tk.END)
                entry.insert(tk.END, str(value))  # Ensure value is converted to string
        else:
            for entry in table2.entries:
                entry.delete(0, tk.END)
                entry.insert(tk.END, "No value in database")

    table2 = Table2(live_data_frame, fetch_live_updated_row())

    headers = [
        "Sr.No.", "Actual Test Pressure\n(Bar)", "Actual Cycle Time\n(min)", "Busting_pressure", "Cycle", "Date & Time"
    ]
    
    class Table2:
        def __init__(self, root, headers, data=None):
            header_width = 25
            data_width = 17

            # Create headers
            for col, header in enumerate(headers):
                self.e = Label(root, text=header, width=header_width, bg='#1C4587', fg='white', font=('Arial', 16, 'bold'), relief='solid', borderwidth=1, justify='center', wraplength=110)
                self.e.grid(row=0, column=col, sticky='nsew', ipady=5)

            # Create data rows if data is provided
            for i in range(1, 11):  # Display top 10 rows
                for j in range(len(headers)):
                    self.e = Entry(root, width=data_width, fg='black', font=('Arial', 12, 'bold'), relief='solid', borderwidth=1, justify='center')
                    self.e.grid(row=i, column=j, sticky='nsew', ipady=5)
                    if data and len(data) > i - 1 and len(data[i - 1]) > j:
                        if j == len(headers) - 1:  # If it's the last column (Date & Time)
                            self.e.insert('end', datetime.now().strftime('%Y-%m-%d %H:%M:%S'))  # Insert current date and time
                        else:
                            self.e.insert('end', data[i - 1][j])  # Insert data from the fetched data

            # Configure grid columns and rows
            for col in range(len(headers)):
                root.grid_columnconfigure(col, weight=1)
            for row in range(11):  # Configure for 11 rows (1 header + 10 data)
                root.grid_rowconfigure(row, weight=1)

    def fetch_head_updated_rows():
        try:
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'

            # Connection string for Windows Authentication
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'

            print(f"Attempting to connect to the SQL Server at {server}...")
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            print("Connection successful.")

            # Fetch the top 10 updated rows
            query = """
            SELECT TOP 10 
                ROW_NUMBER() OVER (ORDER BY insert_datetime DESC) AS SrNo,
                Actual_test_Pressure,
                Actual_cycle_time,
                Busting_pressure,
                cycle,
                insert_datetime  -- Fetch the date and time data as it is
            FROM tbl_new_bust 
            ORDER BY insert_datetime DESC
            """

            cursor.execute(query)
            rows = cursor.fetchall()
            conn.close()

            if rows:
                print("Data fetched successfully from the SQL Server.")
                return rows
            else:
                print("No data returned from the SQL Server.")
                return []
        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)
            return []  # Return empty list in case of an error

    def update_head_Data_table():
        fetched_data = fetch_head_updated_rows()
        if fetched_data:
            data = [list(row) for row in fetched_data]  # Convert fetched_data to a list of lists
            table2 = Table2(data_table_frame, headers, data)  # Pass fetched_data as a list to Table2 constructor
        else:
            print("No data fetched from the SQL Server.")

    update_head_Data_table()
    
    def insert_data(popup, test_no, sample_no):
        try:
            # Step 1: Insert the data into the database
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()
            cursor.execute('''
                INSERT INTO busting_summary (test_no, sample_no) 
                VALUES (?, ?)
            ''', (test_no, sample_no))
            conn.commit()

            # Step 2: Fetch the updated data
            cursor.execute('SELECT * FROM busting_summary ORDER BY insert_datetime DESC')
            fetched_data = cursor.fetchall()

            update_summary_table1()
            popup.destroy()
            start_thread()

        except pyodbc.Error as ex:
            print("An error occurred while inserting data or fetching the updated data:")
            print(f"Error: {ex}")

    def run_executable():
        global process_handle
        executable_path = r"C:\R-axis\dist\Start_stop_file\start_bursting_cycle.bat"
        
        if os.path.isfile(executable_path):
            try:
                # Start the executable and store the process handle
                process_handle = subprocess.Popen([executable_path], shell=True)
                print(f"Executable started with PID: {process_handle.pid}")
            except FileNotFoundError:
                print(f"Executable file not found at path: {executable_path}")
            except Exception as ex:
                print(f"An error occurred while starting the executable: {ex}")
        else:
            print(f"The executable file was not found at path: {executable_path}")

    def start_thread():
        thread = threading.Thread(target=run_executable)
        thread.start()
        

    def update_summary_table1():
        try:
            # Fetch data from the database
            server = 'DESK-4\\SQLEXPRESS'
            database = 'R-AXIS'
            connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes;'
            
            conn = pyodbc.connect(connection_string)
            cursor = conn.cursor()
            
            query = "SELECT TOP 1 test_no, sample_no FROM dbo.busting_summary ORDER BY insert_datetime DESC"
            cursor.execute(query)
            row = cursor.fetchone()
            conn.close()
            
            if row:
                fetched_data = row
            else:
                fetched_data = (None, None)

            # Update the Tkinter Entry widgets
            if fetched_data:
                for entry, value in zip(table1.entries, fetched_data):
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, str(value))
            else:
                for entry in table1.entries:
                    entry.delete(0, tk.END)
                    entry.insert(tk.END, "No value in database")

        except pyodbc.Error as e:
            print("Error fetching data from SQL table:", e)


    # Assuming you have defined the Table class and summary_data_frame somewhere in your code
    table1 = Table(summary_data_frame)
    update_summary_table1()

#------------------------------------------------------------------------------------------------------------------------
# Report button code

def report():
    for widget in main_frame.winfo_children():
        widget.destroy()
        
    # Title Frame
    title_frame = tk.Frame(main_frame, bg="white", height=50)
    title_frame.pack(side="top", fill="x")

    title = tk.Label(title_frame, text='Report', font=('Arial', 20, 'bold'), padx=10, bg="white")
    title.pack(side='left')

    # Body Frame with Border, Background Color, and Center Alignment
    body_frame = tk.Frame(main_frame, bg="#86DBF8", borderwidth=2, relief="solid")
    body_frame.pack(fill=tk.BOTH, expand=True, padx=20, pady=20)

    # Create a frame within body_frame to center align contents
    content_frame = tk.Frame(body_frame, bg="#86DBF8", padx=50, pady=10)
    content_frame.pack(fill=tk.BOTH, expand=True, anchor="center")

    # Create a grid layout for labels and dropdowns within content_frame
    dropdown_label_frame = tk.Frame(content_frame, bg="#86DBF8")
    dropdown_label_frame.grid(row=0, column=0, sticky="ew")

    cycle_label = tk.Label(dropdown_label_frame, text="Select Cycle", font=("Arial", 12), bg="#86DBF8")
    cycle_label.grid(row=0, column=0, padx=(0, 10), sticky="w")

    cycle_var = tk.StringVar()
    sample_no_var = tk.StringVar()
    test_no_var = tk.StringVar()

    cycle_dropdown = ttk.Combobox(dropdown_label_frame, textvariable=cycle_var, font=("Arial", 10))
    cycle_dropdown.grid(row=0, column=1, padx=(0, 20), sticky="ew")
    cycle_dropdown.bind("<<ComboboxSelected>>", lambda event: update_sample_numbers(cycle_var, sample_no_var, sample_no_dropdown))

    # Predefined list of cycle names
    predefined_cycles = [
        "Exploratory Vibration",
        "Variable Vibration",
        "Endurance Vibration Test",
        "Impulse Test",
        "Flexure Fatigue Test",
        "Rotary Fatigue Test",
        "Thermal Cycle Test",
        "Elevated Temperature Soak Test",
        "Hydro Test",
        "Pneumatic Test"
    ]
    cycle_dropdown['values'] = predefined_cycles

    sample_no_label = tk.Label(dropdown_label_frame, text="Sample No", font=("Arial", 12), bg="#86DBF8")
    sample_no_label.grid(row=0, column=2, padx=(0, 10))

    sample_no_dropdown = ttk.Combobox(dropdown_label_frame, textvariable=sample_no_var, font=("Arial", 10))
    sample_no_dropdown.grid(row=0, column=3, padx=(0, 20))
    sample_no_dropdown.bind("<<ComboboxSelected>>", lambda event: update_test_numbers(cycle_var, sample_no_var, test_no_var, test_no_dropdown))

    test_label = tk.Label(dropdown_label_frame, text="Select Test Numbers", font=("Arial", 12), bg="#86DBF8")
    test_label.grid(row=0, column=4, padx=(0, 10))

    test_no_dropdown = ttk.Combobox(dropdown_label_frame, textvariable=test_no_var, font=("Arial", 10))
    test_no_dropdown.grid(row=0, column=5, padx=(0, 20))

    # Create a frame for centering the button
    button_container = tk.Frame(content_frame, bg="#86DBF8")
    button_container.grid(row=1, column=0, columnspan=6, pady=20, sticky="ew")

    # Create and place the "Generate Report" button inside the button_container
    generate_report_btn = tk.Button(button_container, text="Generate Report", font=("Arial", 12), command="on_generate")
    generate_report_btn.pack(padx=50, pady=10)


def update_sample_numbers(cycle_var, sample_no_var, sample_no_dropdown):
    cycle_to_table_mapping = {
        "Exploratory Vibration": "exploratary_summary",
        "Variable Vibration": "variable_summary",
        "Endurance Vibration Test": "endurance_summary",
        "Impulse Test": "Impulse_summary",
        "Flexure Fatigue Test": "flexure_summary",
        "Rotary Fatigue Test": "rotary_summary",
        "Thermal Cycle Test": "theraml_summary",
        "Elevated Temperature Soak Test": "elevated_summary",
        "Hydro Test": "hydro_summary",
        "Pneumatic Test": "pneumatic_summary"
    }

    selected_cycle = cycle_var.get()
    if selected_cycle:
        table_name = cycle_to_table_mapping.get(selected_cycle)
        if not table_name:
            messagebox.showerror("Table Name Error", f"Table name not found for cycle: {selected_cycle}")
            return

        try:
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()

            # Fetch sample_no values
            cursor.execute(f"SELECT DISTINCT sample_no FROM {table_name}")
            sample_numbers = [row.sample_no for row in cursor.fetchall()]

            # Populate sample_no dropdown
            sample_no_dropdown['values'] = sample_numbers
            sample_no_var.set('')  # Clear sample_no selection

        except pyodbc.Error as e:
            messagebox.showerror("Database Error", f"An error occurred: {e}")
            print(f"Database Error: {e}")

        finally:
            if 'cursor' in locals() and cursor:
                cursor.close()
            if 'conn' in locals() and conn:
                conn.close()

    else:
        messagebox.showwarning("Selection Error", "Please select a cycle first")
        sample_no_dropdown['values'] = []
        sample_no_var.set('')  # Clear sample_no selection

def update_test_numbers(cycle_var, sample_no_var, test_no_var, test_no_dropdown):
    cycle_to_table_mapping = {
        "Exploratory Vibration": "exploratary_summary",
        "Variable Vibration": "variable_summary",
        "Endurance Vibration Test": "endurance_summary",
        "Impulse Test": "Impulse_summary",
        "Flexure Fatigue Test": "flexure_summary",
        "Rotary Fatigue Test": "rotary_summary",
        "Thermal Cycle Test": "theraml_summary",
        "Elevated Temperature Soak Test": "elevated_summary",
        "Hydro Test": "hydro_summary",
        "Pneumatic Test": "pneumatic_summary"
    }

    selected_cycle = cycle_var.get()
    selected_sample_no = sample_no_var.get()

    if selected_cycle and selected_sample_no:
        table_name = cycle_to_table_mapping.get(selected_cycle)
        if not table_name:
            messagebox.showerror("Table Name Error", f"Table name not found for cycle: {selected_cycle}")
            return

        try:
            conn = pyodbc.connect(
                'DRIVER={SQL Server};SERVER=DESK-4\\SQLEXPRESS;DATABASE=R-AXIS;Trusted_Connection=yes;'
            )
            cursor = conn.cursor()

            # Fetch test_no values based on selected sample_no
            cursor.execute(f"SELECT DISTINCT test_no FROM {table_name} WHERE sample_no = ?", selected_sample_no)
            test_numbers = [row.test_no for row in cursor.fetchall()]

            # Populate test_no dropdown
            test_no_dropdown['values'] = test_numbers
            test_no_var.set('')  # Clear test_no selection

        except pyodbc.Error as e:
            messagebox.showerror("Database Error", f"An error occurred: {e}")
            print(f"Database Error: {e}")

        finally:
            if 'cursor' in locals() and cursor:
                cursor.close()
            if 'conn' in locals() and conn:
                conn.close()

    else:
        messagebox.showwarning("Selection Error", "Please select a sample number first")
        test_no_dropdown['values'] = []
        test_no_var.set('')  # Clear test_no selection


# Create the main application window
root = tk.Tk()
root.title("R-AXIS")
root.geometry("1200x800")

# Create a main frame to hold the content
main_frame = tk.Frame(root)
main_frame.pack(fill=tk.BOTH, expand=True)#,pady=50

# Make the main_frame responsive
main_frame.grid_rowconfigure(0, weight=1)
main_frame.grid_columnconfigure(0, weight=1)

# Create the menu bar
menu_bar = Menu(root)
root.config(menu=menu_bar)

# Create the 'Home' menu
home_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Home", command=create_home_page)
home_menu.add_command(label="Home", command=lambda: create_home_page(clear_table=True))

# Create the 'Cycles' menu
cycles_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Cycles", menu=cycles_menu)

cycles = [
    'Exploratory Vibration',
    'Variable Vibration', 
    "Endurance Vibration Test", 
    "Impulse Test",
    "Flexure Fatigue Test",
    "Rotary Flexure Test",
    "Thermal Cycle Test",
    "Elevated Temperature Soak Test",
    "Hydro Test",
    "Pneumatic Test",
    "Bust Test"
]

for cycle in cycles:
    cycles_menu.add_command(label=cycle, command=lambda c=cycle: create_page(c))

# Create the 'Reports' menu
reports_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Reports", command=report)
reports_menu.add_command(label="Reports", command=report)

# Create the 'Help' menu
help_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Help", menu=help_menu)
help_menu.add_command(label="Help", command=lambda: create_page("Help"))

# Show the default home page
create_home_page()

#Footer Frame
footer_frame = Frame(root, bg="lightgray", height=50)
footer_frame.pack(side="bottom", fill="x", padx=10, pady=10)

footer_label = Button(footer_frame, text="Logged in as: Admin | Logout", bg="#1C4587", fg="white", anchor="e", command=logout)
footer_label.pack(side="left")

copyright_label = Label(footer_frame, text="Copyright © www.Redogroup.in", bg="#1C4587", fg="white", anchor="e", cursor="hand2")
copyright_label.pack(side="right")

copyright_label.bind("<Button-1>", lambda event: open_link())

# Run the application
root.mainloop()
