import streamlit as st
import pandas as pd
from datetime import datetime, timedelta

# Inject custom CSS for the background
def set_background_image(image_url):
    st.markdown(
        f"""
        <style>
        body {{
            background-image: url("{image_url}");
            background-size: auto;
            background-repeat: no-repeat;
            background-attachment: fixed;
            background-position: center;
            color: black;
        }}
        </style>
        """,
        unsafe_allow_html=True,
    )

# Set the background image
set_background_image("C:\\Users\\M AMRUTH KUMAR JAIN\\OneDrive\\Desktop\\download.jpeg")

# Initialize library data
if "library_data" not in st.session_state:
    st.session_state.library_data = []

if "checked_out_items" not in st.session_state:
    st.session_state.checked_out_items = []

# Function to add a new item to the library
def add_item(title, author, category, item_type):
    st.session_state.library_data.append({
        "Title": title,
        "Author": author,
        "Category": category,
        "Type": item_type,
        "Available": True
    })

# Function to check out an item
def check_out_item(title, user_name):
    for item in st.session_state.library_data:
        if item["Title"].lower() == title.lower() and item["Available"]:
            item["Available"] = False
            st.session_state.checked_out_items.append({
                "Title": title,
                "User": user_name,
                "Checkout Date": datetime.now(),
                "Due Date": datetime.now() + timedelta(days=14),
            })
            return True
    return False

# Function to return an item
def return_item(title):
    for item in st.session_state.checked_out_items:
        if item["Title"].lower() == title.lower():
            st.session_state.checked_out_items.remove(item)
            for lib_item in st.session_state.library_data:
                if lib_item["Title"].lower() == title.lower():
                    lib_item["Available"] = True
            return True
    return False

# Function to search for items
def search_items(keyword):
    return [
        item for item in st.session_state.library_data
        if keyword.lower() in item["Title"].lower()
        or keyword.lower() in item["Author"].lower()
        or keyword.lower() in item["Category"].lower()
    ]

# Streamlit UI
st.title("Library Management System")

menu = st.sidebar.selectbox("Menu", ["Add Item", "Check Out Item", "Return Item", "Search Items", "Overdue Fines"])

if menu == "Add Item":
    st.header("Add a New Item")
    title = st.text_input("Title")
    author = st.text_input("Author")
    category = st.text_input("Category")
    item_type = st.selectbox("Type", ["Book", "Magazine", "DVD"])
    if st.button("Add Item"):
        add_item(title, author, category, item_type)
        st.success(f"Item '{title}' added successfully!")

elif menu == "Check Out Item":
    st.header("Check Out an Item")
    title = st.text_input("Item Title")
    user_name = st.text_input("User Name")
    if st.button("Check Out"):
        if check_out_item(title, user_name):
            st.success(f"Item '{title}' checked out successfully!")
        else:
            st.error(f"Item '{title}' is not available!")

elif menu == "Return Item":
    st.header("Return an Item")
    title = st.text_input("Item Title")
    if st.button("Return"):
        if return_item(title):
            st.success(f"Item '{title}' returned successfully!")
        else:
            st.error(f"Item '{title}' not found in checked-out items!")

elif menu == "Search Items":
    st.header("Search Library Items")
    keyword = st.text_input("Search by Title, Author, or Category")
    if keyword:
        results = search_items(keyword)
        if results:
            st.write(pd.DataFrame(results))
        else:
            st.warning("No items found!")

elif menu == "Overdue Fines":
    st.header("Manage Overdue Fines")
    overdue_items = [
        item for item in st.session_state.checked_out_items
        if item["Due Date"] < datetime.now()
    ]
    if overdue_items:
        st.write("Overdue Items:")
        st.write(pd.DataFrame(overdue_items))
    else:
        st.info("No overdue items.")
