
목표는 다음과 같이 요약할 수 있습니
![:흰색_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/2705.png) 목표 시스템 구조  

[Client (Next.js)]
      ↓ (REST API)
[FastAPI 서버] ←─ PDF 업로드
      ↓
[텍스트 + 표 추출] ← PyMuPDF/pdfplumber
      ↓
[LLM 요약 or 임베딩] ← LangChain + Llama + RAG
      ↓
[Supabase 저장 (벡터 + 원문)]
      ↓
[다시 Next.js에서 API 호출 → 내용 전달]

![:흰색_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/2705.png) 주요 기술스택 요약  
   구성 요소 도구     백엔드 FastAPI   LLM/RAG LangChain + Llama (로컬 or OpenRouter)   PDF 처리 PyMuPDF / pdfplumber   벡터 DB Supabase Vector or ChromaDB   프론트 Next.js 15   API 구조 RESTful (`/upload`, `/query`, `/summary`)  
![:일:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/0031-fe0f-20e3.png) FastAPI: PDF 업로드 & 추출  

pip install fastapi uvicorn pdfplumber langchain openai supabase
# main.py
from fastapi import FastAPI, File, UploadFile
import pdfplumber
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import SupabaseVectorStore
from supabase import create_client

app = FastAPI()

SUPABASE_URL = "[https://your-project.supabase.co](https://your-project.supabase.co/)"
SUPABASE_KEY = "your-secret-api-key"
supabase = create_client(SUPABASE_URL, SUPABASE_KEY)

@app.post("/upload")
async def upload_pdf(file: UploadFile = File(...)):
    # PDF에서 텍스트 및 표 추출
    with pdfplumber.open(file.file) as pdf:
        texts, tables = [], []
        for page in pdf.pages:
            texts.append(page.extract_text())
            table = page.extract_table()
            if table:
                tables.append(table)

    raw_text = "\n".join(texts)

    # LLM 기반 요약 or 벡터 임베딩 (LangChain)
    # 아래는 예시용 코드 (임베딩 → Supabase Vector 저장)
    embeddings = HuggingFaceEmbeddings(model_name="intfloat/multilingual-e5-base")
    vector_store = SupabaseVectorStore(supabase_client=supabase, embedding=embeddings)

    # 문서 저장
    vector_store.add_texts([raw_text], metadatas=[{"source": file.filename}])

    return {"status": "uploaded", "source": file.filename}

![:둘:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/0032-fe0f-20e3.png) RAG 기반 문서 Q&A (LLM + LangChain)  

from langchain.chains import RetrievalQA
from langchain.llms import LlamaCpp  # or OpenAI, HuggingFaceHub
from langchain.vectorstores import SupabaseVectorStore

@app.get("/query")
def query_document(question: str):
    vector_store = SupabaseVectorStore(supabase_client=supabase, embedding=embeddings)

    retriever = vector_store.as_retriever()
    llm = LlamaCpp(model_path="path/to/llama.gg", n_ctx=4096)

    qa = RetrievalQA.from_chain_type(llm=llm, retriever=retriever, chain_type="stuff")
    answer = qa.run(question)
    return {"answer": answer}

> 위 LlamaCpp는 CPU에서도 돌릴 수 있고, Llama2, TinyLlama 등 HuggingFace에서 선택 가능  
>  만약 Supabase Vector 사용이 어렵다면 → ChromaDB, FAISS로 교체 가능

![:셋:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/0033-fe0f-20e3.png) Next.js (클라이언트) 예시  

// /pages/index.tsx
export default function Home() {
  const upload = async (file: File) => {
    const form = new FormData()
    form.append("file", file)
    await fetch("[https://your-api/upload](https://your-api/upload)", { method: "POST", body: form })
  }

  const ask = async (q: string) => {
    const res = await fetch(`[https://your-api/query?question=${q}](https://your-api/query?question=${q})`)
    const { answer } = await res.json()
    console.log("답변:", answer)
  }

  return (
    <div>
      <input type="file" onChange={(e) => upload(e.target.files[0])} />
      <button onClick={() => ask("이 보고서의 핵심 내용은?")}>질문하기</button>
    </div>
  )
}

![:렌치:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/1f527.png) 배포 방법 (Railway + Vercel)  
   구성 배포 방법     FastAPI + LangChain + Supabase SDK Docker → Railway   Next.js Vercel 자동 배포   Supabase 벡터 DB 구성 후 URL & Key 연결  
![:선물:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/1f381.png) 요약  
   단계 설명     1 FastAPI에서 PDF 텍스트/표 추출   2 LangChain으로 요약 또는 문서 임베딩   3 Supabase에 저장 (벡터 + 메타)   4 Next.js에서 REST API 호출로 질문/응답  
원하시면 위 코드를 **구현 가능한 폴더 구조 + Dockerfile + Railway 배포 설정**으로 정리해드릴 수 있습니다.  
 또는 이 구조를 TaskMaster 기반 PRD로도 변환해드릴까요?