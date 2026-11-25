# Smart Firm Archiver

AI asistent v konzoli, který odpovídá na dotazy nad interními dokumenty firmy (popisy projektů a jejich rozpočty) a umí k rozpočtu dopočítat DPH.

Výukový projekt, na kterém jsem si zkoušel RAG a AI agenty s nástroji (tool calling).

## Jak to funguje
1. Textové soubory ve složce `data/` se rozdělí na menší části a uloží do vektorové databáze **ChromaDB** (embeddingy z HuggingFace).
2. Agent postavený v **LangGraph** dostane dotaz a sám rozhodne, jestli zavolá nástroj:
   - `get_project_info`: najde v databázi relevantní dokumenty (RAG),
   - `calculate_tax`: připočítá 21 % DPH.
3. Odpověď generuje model Llama 3.1 přes **Groq API**. Konverzace si pamatuje kontext (`MemorySaver`).

Příklad dotazu: *„Jaký je rozpočet projektu Gama včetně DPH?“*

## Technologie
Python, LangGraph, LangChain, ChromaDB, HuggingFace Embeddings, Groq API, regulární výrazy

## Spuštění
Potřebuješ Python 3.10+ a zdarma API klíč z https://console.groq.com.

```bash
git clone https://github.com/daniel-becher/smart-firm-archiver.git
cd smart-firm-archiver
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # a do .env vlož svůj GROQ_API_KEY
python smart_firm_archiver.py
```

Konverzaci ukončíš napsáním `exit`.

Poznámka: při prvním spuštění se stahuje embedding model (cca 500 MB).
