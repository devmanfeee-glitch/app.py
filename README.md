import streamlit as st
import google.generativeai as genai

# إعداد واجهة التطبيق
st.set_page_config(page_title="مساعدي الذكي", layout="centered")
st.title("🤖 مساعدي الشخصي للذكاء الاصطناعي")

# إدخال مفتاح API (يمكنك وضعه مباشرة هنا أو في الإعدادات)
api_key = "ضع_مفتاح_الـ_API_الخاص_بك_هنا"
genai.configure(api_key=api_key)

# اختيار المهمة
option = st.selectbox(
    "ماذا تريد أن نفعل اليوم؟",
    ("كتابة مقال طويل", "إعادة صياغة نص", "تصحيح لغوي", "مساعد عام")
)

# مدخلات المستخدم
user_input = st.text_area("أدخل التفاصيل أو النص هنا:", height=200)

if st.button("توليد النص"):
    if user_input:
        model = genai.GenerativeModel('gemini-pro')
        
        # تخصيص الأمر بناءً على الاختيار
        if option == "كتابة مقال طويل":
            prompt = f"اكتب مقالاً مفصلاً واحترافياً حول: {user_input}"
        elif option == "إعادة صياغة نص":
            prompt = f"أعد صياغة النص التالي بأسلوب بليغ وجذاب: {user_input}"
        else:
            prompt = user_input

        with st.spinner('جاري التفكير...'):
            response = model.generate_content(prompt)
            st.markdown("### النتيجة:")
            st.write(response.text)
    else:
        st.warning("يرجى إدخال نص أولاً!")
