import streamlit as st
import pandas as pd

# Initialize session state variables
if "businesses" not in st.session_state:
    st.session_state.businesses = {}

# Function to add a new business
def add_business(name, fixed_expenses):
    if name in st.session_state.businesses:
        st.warning(f"Business '{name}' already exists.")
    else:
        st.session_state.businesses[name] = {
            "fixed_expenses": fixed_expenses,
            "monthly_data": {}
        }
        st.success(f"Business '{name}' added successfully!")

# Function to add monthly data
def add_monthly_data(business_name, month, variable_expenses, revenue):
    if business_name not in st.session_state.businesses:
        st.error(f"Business '{business_name}' not found.")
        return
    
    business = st.session_state.businesses[business_name]
    total_fixed = sum(business["fixed_expenses"].values())
    total_variable = sum(variable_expenses.values())
    net_balance = revenue - (total_fixed + total_variable)
    
    # Store monthly data
    business["monthly_data"][month] = {
        "Fixed Expenses": total_fixed,
        "Variable Expenses": total_variable,
        "Revenue": revenue,
        "Net Balance": net_balance,
    }
    st.success(f"Data for {month} added to '{business_name}'.")

# Streamlit App Interface
st.title("Business Expense Tracker")

# Add New Business
st.header("Add New Business")
business_name = st.text_input("Business Name")
fixed_expenses_input = st.text_area("Fixed Expenses (name:amount)", placeholder="e.g., Rent:5000\nUtilities:2000")
if st.button("Add Business"):
    if fixed_expenses_input.strip():
        fixed_expenses = {item.split(":")[0]: float(item.split(":")[1]) for item in fixed_expenses_input.split("\n")}
        add_business(business_name, fixed_expenses)
    else:
        st.error("Please enter valid fixed expenses.")

# Select Business and Add Monthly Data
st.header("Add Monthly Data")
selected_business = st.selectbox("Select Business", list(st.session_state.businesses.keys()))
month = st.text_input("Month (e.g., 01/2025)")
variable_expenses_input = st.text_area("Variable Expenses (name:amount)", placeholder="e.g., Marketing:3000\nRepairs:500")
revenue = st.number_input("Monthly Revenue", min_value=0.0, step=100.0)
if st.button("Add Monthly Data"):
    if variable_expenses_input.strip():
        variable_expenses = {item.split(":")[0]: float(item.split(":")[1]) for item in variable_expenses_input.split("\n")}
        add_monthly_data(selected_business, month, variable_expenses, revenue)
    else:
        st.error("Please enter valid variable expenses.")

# Display Summary for Selected Business
st.header("View Summary")
if selected_business:
    business_data = st.session_state.businesses[selected_business]
    if business_data["monthly_data"]:
        df_summary = pd.DataFrame.from_dict(business_data["monthly_data"], orient="index")
        df_summary.index.name = "Month"
        st.write(f"Summary for '{selected_business}':")
        st.dataframe(df_summary)
    else:
        st.info(f"No monthly data available for '{selected_business}'.")
