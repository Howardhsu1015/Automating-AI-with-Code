# Automating-AI-with-Code
# 安裝 Gemini 套件（只需第一次執行）
!pip install -q -U google-generativeai

# 匯入函式庫
import google.generativeai as genai
from google.colab import userdata
import os
import time

# 讀取金鑰
api_key = userdata.get('GEMINI_API_KEY')

if not api_key:
    print("❌ 無法讀取 GEMINI_API_KEY，請先在左邊 🔑 輸入你的 API 金鑰")
else:
    print(f"🔑 已讀取金鑰（部分遮蔽）：{api_key[:4]}...{api_key[-4:]}")
    genai.configure(api_key=api_key)

    # ⚙️ 設定模型（可改為 gemini-1.5-pro）
    MODEL_NAME = "models/gemini-1.5-flash"

    # ✅ 控制是否真的要發送 prompt
    run_prompt = True

    # ✅ 你要問的問題
    prompt = "請幫我寫一段 Python 最好的入門方法"

    # ✅ 回應快取檔名
    CACHE_FILE = "gemini_response.txt"

    if os.path.exists(CACHE_FILE):
        print(f"📄 已有快取，以下是之前的回覆：\n")
        with open(CACHE_FILE, "r", encoding="utf-8") as f:
            print(f.read())
    elif run_prompt:
        try:
            model = genai.GenerativeModel(model_name=MODEL_NAME)
            print("🧠 正在呼叫 Gemini API...")
            time.sleep(2)  # 避免429錯誤

            response = model.generate_content(prompt)
            answer = response.text

            print("✅ Gemini 回覆：\n", answer)

            with open(CACHE_FILE, "w", encoding="utf-8") as f:
                f.write(answer)
        except Exception as e:
            print("⚠️ 發生錯誤：", e)
    else:
        print("🛑 測試模式中，未發送 API 請求。請將 run_prompt 改為 True。")
