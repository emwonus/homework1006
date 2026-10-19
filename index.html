import datetime
from bs4 import BeautifulSoup
import requests
import streamlit as st

st.set_page_config(
    page_title="K-MONSTAR 銷量與庫存監控", page_icon="⭐", layout="centered"
)

if "ongoing_events" not in st.session_state:
  st.session_state.ongoing_events = []

st.title("⭐ K-MONSTAR 銷量與庫存監控")

tab1, tab2 = st.tabs(["🔥 進行中活動監控", "🏁 查詢已結束/任意活動"])


def fetch_event_data(url):
  headers = {
      "User-Agent": (
          "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
          " (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36"
      )
  }
  try:
    res = requests.get(url, headers=headers, timeout=5)
    if res.status_code == 200:
      soup = BeautifulSoup(res.text, "html.parser")

      # 1. 抓取數量（Shopify 負數庫存）
      target = soup.find(attrs={"data-inventory_quantity": True})
      qty = int(target.get("data-inventory_quantity")) if target else None

      # 2. 自動抓取標題（優先找 og:title，其次 h1，最後 title）
      title = "未知活動"
      og_title = soup.find("meta", property="og:title")
      if og_title and og_title.get("content"):
        title = og_title.get("content")
      else:
        h1_tag = soup.find("h1")
        if h1_tag:
          title = h1_tag.get_text(strip=True)
        elif soup.title:
          title = soup.title.string.strip()

      return qty, title
  except:
    pass
  return None, "未知活動"


with tab1:
  with st.form("add_event_form", clear_on_submit=True):
    event_url = st.text_input(
        "輸入活動網址（系統會自動抓取標題與庫存）",
        placeholder="https://www.kmonstar.com.tw/products/...",
    )
    submitted = st.form_submit_button("新增活動並自動監控", type="primary")
    if submitted and event_url:
      initial_qty, auto_name = fetch_event_data(event_url)
      st.session_state.ongoing_events.append({
          "name": auto_name,
          "url": event_url,
          "last_qty": initial_qty,
          "logs": [],
      })
      st.success(f"成功自動新增：{auto_name}")

  st.markdown("---")

  refresh_seconds = st.slider(
      "自動更新頻率（秒）", min_value=1, max_value=30, value=2
  )


  @st.fragment(run_every=refresh_seconds)
  def render_ongoing_events():
    if not st.session_state.ongoing_events:
      st.info("目前沒有進行中的活動，請在上方新增網址。")
      return

    for idx, ev in enumerate(st.session_state.ongoing_events):
      current_qty, _ = fetch_event_data(ev["url"])
      now_time = datetime.datetime.now().strftime("%H:%M:%S")

      if current_qty is not None:
        if ev["last_qty"] is None:
          ev["last_qty"] = current_qty
        elif current_qty != ev["last_qty"]:
          # 負數往下掉代表售出，計算絕對值差額
          sold_count = abs(current_qty) - abs(ev["last_qty"])
          if sold_count > 0:
            ev["logs"].append({
                "time": now_time,
                "old": abs(ev["last_qty"]),
                "new": abs(current_qty),
                "sold": sold_count,
            })
          ev["last_qty"] = current_qty

      current_total_sold = (
          abs(ev["last_qty"]) if ev["last_qty"] is not None else 0
      )

      with st.expander(
          f"🔥 {ev['name']} | 目前總售出：{current_total_sold} 張",
          expanded=True,
      ):
        st.caption(f"網址：{ev['url']}")

        col1, col2 = st.columns(2)
        with col1:
          st.metric(
              label="原始庫存數據",
              value=ev["last_qty"] if ev["last_qty"] is not None else "讀取中",
          )
        with col2:
          st.metric(label="即時總銷量", value=f"{current_total_sold} 張")

        if st.button("🗑️ 移除此監控", key=f"remove_{idx}"):
          st.session_state.ongoing_events.pop(idx)
          st.rerun()

        if ev["logs"]:
          st.markdown("📈 **訂單變動紀錄：**")
          for log in reversed(ev["logs"]):
            st.text(
                f"[{log['time']}] 🛒 瞬間售出 {log['sold']} 張 (總銷量變化:"
                f" {log['old']} ➡️ {log['new']})"
            )
        else:
          st.caption("⏳ 監控中，靜待下單變動...")


  render_ongoing_events()

with tab2:
  st.markdown("**🏁 查詢已結束或任意活動總銷量**")
  st.write(
      "直接貼上任何 K-MONSTAR 活動網址，系統會自動辨識標題並計算最終總銷量。"
  )

  query_url = st.text_input("輸入要查詢的活動網址", key="query_url_input")
  if st.button("🔍 立即查詢總銷量", type="primary"):
    if not query_url:
      st.warning("請先輸入網址！")
    else:
      with st.spinner("正在抓取資料與標題中..."):
        res_qty, res_title = fetch_event_data(query_url)
        if res_qty is not None:
          total_sold = abs(res_qty)
          st.success(f"查詢成功：{res_title}")
          st.metric(label="🏆 該活動最終總銷量", value=f"{total_sold} 張")
          st.caption(f"原始抓取數值：{res_qty}")
        else:
          st.error("無法取得該網址的數據，請確認網址是否正確。")