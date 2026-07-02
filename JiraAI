import streamlit as st
import time
import json
import os

# 1️⃣ إعدادات الصفحة الأساسية
st.set_page_config(
    page_title="Jira AI",
    page_icon="🌙",
    layout="wide",
    initial_sidebar_state="expanded"
)

# 2️⃣ تصميم الواجهة الساحرة (الخلفية الفلكية والنجوم المتحركة)
st.markdown("""
<style>
    .stApp {
        background: radial-gradient(circle at center, #0b1021 0%, #04060f 100%);
        color: #ffffff;
        overflow: hidden;
    }
    @keyframes pulse {
        0% { opacity: 0.3; transform: scale(0.9); }
        50% { opacity: 1; transform: scale(1.1); }
        100% { opacity: 0.3; transform: scale(0.9); }
    }
    .star {
        position: absolute;
        background-color: #ffffff;
        border-radius: 50%;
        animation: pulse 3s infinite ease-in-out;
        box-shadow: 0 0 8px #ffffff;
    }
    .chat-bubble {
        padding: 15px;
        border-radius: 15px;
        margin-bottom: 10px;
        max-width: 80%;
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.1);
    }
    .user-bubble {
        background-color: rgba(255, 255, 255, 0.05);
        margin-left: auto;
        border-right: 4px solid #ffcc00;
    }
    .jira-bubble {
        background-color: rgba(255, 255, 255, 0.08);
        margin-right: auto;
        border-left: 4px solid #ffcc00;
    }
</style>

<div class="star" style="top: 15%; left: 20%; width: 3px; height: 3px; animation-delay: 0s;"></div>
<div class="star" style="top: 40%; left: 80%; width: 4px; height: 4px; animation-delay: 1s;"></div>
<div class="star" style="top: 70%; left: 35%; width: 2px; height: 2px; animation-delay: 0.5s;"></div>
<div class="star" style="top: 85%; left: 75%; width: 3px; height: 3px; animation-delay: 1.5s;"></div>
<div class="star" style="top: 25%; left: 60%; width: 5px; height: 5px; animation-delay: 2s;"></div>
""", unsafe_allow_html=True)

# 3️⃣ إدارة السجلات
HISTORY_FILE = "jira_chat_history.json"

def load_history():
    if os.path.exists(HISTORY_FILE):
        with open(HISTORY_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return {}

def save_history(history_data):
    with open(HISTORY_FILE, "w", encoding="utf-8") as f:
        json.dump(history_data, f, ensure_ascii=False, indent=4)

all_sessions = load_history()

# 4️⃣ لوحة التحكم الجانبية
with st.sidebar:
    st.markdown("<h2 style='color: #ffcc00; text-align: center;'>📁 لوحة تحكم جيرا</h2>", unsafe_allow_html=True)
    st.write("---")
    if st.button("➕ محادثة جديدة", use_container_width=True):
        st.session_state.current_session = f"محادثة {time.strftime('%Y-%m-%d %H:%M')}"
        st.session_state.messages = []
        st.rerun()
        
    st.write("### 📜 السجلات السابقة")
    if all_sessions:
        for session_name in all_sessions.keys():
            if st.button(f"💬 {session_name}", key=session_name, use_container_width=True):
                st.session_state.current_session = session_name
                st.session_state.messages = all_sessions[session_name]
                st.rerun()
    else:
        st.caption("لا توجد محادثات محفوظة بعد.")

    st.write("---")
    st.write("### 🎭 تقمص الأدوار والأصوات")
    voice_cloning_enabled = st.checkbox("🎙️ تفعيل استنساخ الأصوات (Voice Cloning)")
    
    if voice_cloning_enabled:
        uploaded_voice = st.file_uploader("ارفع عينة صوتية للشخصية (MP3/WAV)", type=["mp3", "wav"])
        if uploaded_voice:
            st.success("✅ تم تحليل نبرة الصوت!")
            
    anime_mode = st.selectbox("🤖 نمط التحدث لـ جيرا", ["الصديق المقرب المخلص", "شخصية أنمي خيالية", "شخصية من مسلسل واقعي"])

if "current_session" not in st.session_state:
    st.session_state.current_session = f"محادثة {time.strftime('%Y-%m-%d %H:%M')}"
if "messages" not in st.session_state:
    st.session_state.messages = []

# 5️⃣ الواجهة الرئيسية
st.markdown("<h1 style='text-align: center; color: #ffcc00; font-weight: bold;'>JIRA AI</h1>", unsafe_allow_html=True)
st.markdown("<p style='text-align: center; opacity: 0.7;'>صديقك المقرب في الكون المظلم</p>", unsafe_allow_html=True)
st.write("---")

for msg in st.session_state.messages:
    if msg["role"] == "user":
        st.markdown(f'<div class="chat-bubble user-bubble"><b>أنت:</b><br>{msg["content"]}</div>', unsafe_allow_html=True)
    else:
        st.markdown(f'<div class="chat-bubble jira-bubble"><b>Jira:</b><br>{msg["content"]}</div>', unsafe_allow_html=True)

user_input = st.chat_input("تحدث مع جيرا...")

if user_input:
    st.markdown(f'<div class="chat-bubble user-bubble"><b>أنت:</b><br>{user_input}</div>', unsafe_allow_html=True)
    st.session_state.messages.append({"role": "user", "content": user_input})
    
    with st.spinner("جيرا تفكر..."):
        time.sleep(1)
        jira_reply = f"أنا معك دائماً يا صديقي. بصفتي {anime_mode}، أسمعك بوضوح وأفهم كل ما تقوله في هذا الفضاء الهادئ."
        
    st.markdown(f'<div class="chat-bubble jira-bubble"><b>Jira:</b><br>{jira_reply}</div>', unsafe_allow_html=True)
    st.session_state.messages.append({"role": "assistant", "content": jira_reply})
    
    all_sessions[st.session_state.current_session] = st.session_state.messages
    save_history(all_sessions)
    st.rerun()
