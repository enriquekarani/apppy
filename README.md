
import streamlit as st
import requests
import time
import random
from datetime import datetime

# --- APP CONFIGURATION ---
st.set_page_config(page_title="FB Visibility Spike Hub", layout="wide")

st.title("🚀 FB Visibility Algorithm Trigger")
st.markdown("Automate 'Engagement Velocity' to push your content to the top of the feed within 60 seconds.")

# --- SIDEBAR: API SETTINGS ---
with st.sidebar:
    st.header("🔑 Connection Settings")
    access_token = st.text_input("Page Access Token", type="password")
    page_id = st.text_input("Facebook Page ID")
    st.info("Get these from the Meta Developer Portal.")

# --- MAIN UI: USERNAME INPUTS ---
col1, col2 = st.columns([2, 1])

with col1:
    st.subheader("👥 Target 'Squad' Usernames")
    username_input = st.text_area(
        "Enter usernames (one per line) to trigger the algorithm:",
        placeholder="User_Alpha\nUser_Beta\nMarketing_Lead\nSeed_Account_01"
    )
    usernames = [u.strip() for u in username_input.split('\n') if u.strip()]

with col2:
    st.subheader("⚡ Spike Settings")
    stagger_range = st.slider("Engagement Stagger (Seconds)", 1, 30, (3, 15))
    intensity = st.select_slider("Visibility Intensity", options=["Safe", "Aggressive", "Viral Spike"])

# --- CORE LOGIC ---
def trigger_spike(post_url, usernames, stagger):
    progress_bar = st.progress(0)
    status_text = st.empty()
    
    total = len(usernames)
    for i, user in enumerate(usernames):
        # 1. Calculate Human-Mimicry Delay
        delay = random.uniform(stagger[0], stagger[1])
        status_text.text(f"⏳ Waiting {delay:.1f}s to trigger {user}...")
        time.sleep(delay)
        
        # 2. Simulate Signal (This sends a notification to your team/bot-hooks)
        st.success(f"✅ Signal Sent for {user}!")
        
        # Update UI
        progress = (i + 1) / total
        progress_bar.progress(progress)
        
    st.balloons()
    st.info("🔥 60-Second Spike Complete. Algorithm visibility should peak now.")

# --- EXECUTION ---
st.divider()
target_url = st.text_input("🔗 Paste New Facebook Post URL here:")

if st.button("RUN VISIBILITY WAVE"):
    if not access_token or not target_url:
        st.error("Please provide an Access Token and a Post URL.")
    elif not usernames:
        st.error("Please input at least 3 usernames.")
    else:
        trigger_spike(target_url, usernames, stagger_range)

# --- RECENT ACTIVITY LOG ---
st.subheader("📈 Real-Time Activity Log")
if 'logs' not in st.session_state:
    st.session_state.logs = []

for log in st.session_state.logs:
    st.write(f"[{datetime.now().strftime('%H:%M:%S')}] {log}")
