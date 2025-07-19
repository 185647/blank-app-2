
   `import streamlit as st
import yfinance as yf
import datetime

st.set_page_config(page_title="Stock & Crypto Analyzer", layout="wide")
st.title("📈 Stock and Crypto Analyzer")
st.markdown("הזן סמל מניה או מטבע (כמו 'AAPL', 'GOOG', 'BTC-USD')")

symbol = st.text_input("סימול:", "AAPL")
start_date = st.date_input("תאריך התחלה:", datetime.date.today() - datetime.timedelta(days=365*5))
end_date = st.date_input("תאריך סיום:", datetime.date.today())

if st.button("נתח"):
    with st.spinner("טוען נתונים..."):
        try:
            data = yf.download(symbol, start=start_date, end=end_date)
            if data.empty:
                st.error("לא נמצאו נתונים. בדוק את הסימול.")
            else:
                st.success(f"נמצאו {len(data)} ימי מסחר עבור {symbol}")
                st.line_chart(data["Adj Close"], use_container_width=True)
                st.subheader("תיאור סטטיסטי")
                st.dataframe(data.describe())
        except Exception as e:
            st.error(f"שגיאה: {e}")
``
