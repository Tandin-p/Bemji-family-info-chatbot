import streamlit as st
import pandas as pd

# Load the Excel file
@st.cache_data
def load_data():
    df = pd.read_excel("f8dfbb59-a863-479e-bec1-9dc98f3d0055.xlsx")
    df.columns = df.columns.str.lower().str.strip()
    return df

df = load_data()

# App title
st.title("👨‍👩‍👧‍👦 Family Info Chatbot")

# User input
question = st.text_input("Ask about a family member (e.g., 'What is Karma's phone number?')")

# Process input and respond
if question:
    found = False
    for _, row in df.iterrows():
        name = row['name'].strip().lower()
        if name in question.lower():
            response_parts = []
            if "cid" in question.lower():
                response_parts.append(f"CID: {row['cid']}")
            if "house" in question.lower():
                response_parts.append(f"House No: {row['house no']}")
            if "land" in question.lower():
                response_parts.append(f"Land No: {row['land no']}")
            if "phone" in question.lower():
                response_parts.append(f"Phone No: {row['phone no']}")

            if not response_parts:
                response_parts = [
                    f"CID: {row['cid']}",
                    f"House No: {row['house no']}",
                    f"Land No: {row['land no']}",
                    f"Phone No: {row['phone no']}"
                ]

            st.success(f"{row['name']}\n" + "\n".join(response_parts))
            found = True
            break

    if not found:
        st.warning("Sorry, I couldn't find that family member.")
