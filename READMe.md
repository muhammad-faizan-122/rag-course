# [Retrieval Augmented Generation (RAG) Coursera (By Zain Hasan)](https://www.coursera.org/learn/retrieval-augmented-generation-rag)
RAG course is paid course available on Coursera taught Zain Hasan. The course is consist of four modules. This repo contains the lecture slides, lecture notes, quizes, assignment, and lab involving in each module.

## project Structure
```
.
├── module-1
│   ├── notebooks
│   │   ├── assignment
│   │   │   ├── C1M1_Assignment.ipynb
│   │   │   ├── embeddings.joblib
│   │   │   ├── images
│   │   │   │   ├── rag_overview.png
│   │   │   │   └── toc.png
│   │   │   ├── news_data_dedup.csv
│   │   │   ├── unittests.py
│   │   │   └── utils.py
│   │   └── labs
│   │       ├── C1M1_Ungraded_Lab_1.ipynb
│   │       ├── C1M1_Ungraded_Lab_2.ipynb
│   │       └── utils.py
│   ├── notes
│   │   └── lecture_notes.md
│   ├── quiz
│   │   ├── p1.png
│   │   ├── p2.png
│   │   ├── p3.png
│   │   └── p4.png
│   └── slides
│       └── RAG_M1.pdf
├── module-2
│   ├── notebooks
│   │   ├── assignment
│   │   │   ├── C1M2_Assignment.ipynb
│   │   │   ├── embeddings.joblib
│   │   │   ├── images
│   │   │   │   ├── retriever_overview.png
│   │   │   │   └── toc.png
│   │   │   ├── news_data_dedup.csv
│   │   │   ├── unittests.py
│   │   │   └── utils.py
│   │   └── labs
│   │       ├── 1
│   │       │   ├── C1M2_Ungraded_Lab_1.ipynb
│   │       │   ├── embeddings.joblib
│   │       │   ├── images
│   │       │   │   ├── cosine.png
│   │       │   │   ├── documents.png
│   │       │   │   ├── embedding.png
│   │       │   │   ├── euclidean.png
│   │       │   │   ├── precision_recall.png
│   │       │   │   ├── toc.png
│   │       │   │   └── vector_space.png
│   │       │   ├── large_text.txt
│   │       │   └── utils.py
│   │       └── 2
│   │           ├── C1M2_Ungraded_Lab_2.ipynb
│   │           └── images
│   │               ├── cosine.png
│   │               ├── documents.png
│   │               ├── embedding.png
│   │               ├── euclidean.png
│   │               ├── precision_recall.png
│   │               ├── toc.png
│   │               └── vector_space.png
│   ├── notes
│   │   └── lecture_notes.md
│   ├── quiz
│   │   ├── 1.png
│   │   ├── 2.png
│   │   └── 3.png
│   └── slides
│       └── RAG_M2.pdf
├── module-3
│   ├── notebooks
│   │   ├── assignment
│   │   │   ├── C1M3_Assignment.ipynb
│   │   │   ├── data
│   │   │   │   └── bbc_data.joblib
│   │   │   ├── flask_app.py
│   │   │   ├── images
│   │   │   │   └── toc.png
│   │   │   ├── unittests.py
│   │   │   ├── utils.py
│   │   │   └── weaviate_server.py
│   │   └── labs
│   │       ├── 1
│   │       │   ├── C1M3_Ungraded_Lab_1.ipynb
│   │       │   ├── data.joblib
│   │       │   ├── flask_app.py
│   │       │   ├── images
│   │       │   │   ├── toc.png
│   │       │   │   ├── vector_db.png
│   │       │   │   ├── weaviate.png
│   │       │   │   └── workflow.png
│   │       │   └── utils.py
│   │       └── 2
│   │           ├── C1M3_Ungraded_Lab_2.ipynb
│   │           ├── data.joblib
│   │           ├── flask_app.py
│   │           ├── images
│   │           │   ├── chunking.png
│   │           │   ├── chunking_type.png
│   │           │   ├── fixed_size.png
│   │           │   ├── overlap.png
│   │           │   └── recursive.png
│   │           └── utils.py
│   ├── notes
│   │   └── lecture_notes.md
│   ├── quiz
│   │   ├── 1.png
│   │   ├── 2.png
│   │   └── 3.png
│   └── slides
│       └── RAG_M3.pdf
├── module-4
│   ├── notebooks
│   │   ├── assignment
│   │   │   ├── C1M4_Assignment.ipynb
│   │   │   ├── clothing_ft_format.csv
│   │   │   ├── data
│   │   │   │   ├── clothes.csv
│   │   │   │   ├── clothes_json.joblib
│   │   │   │   └── faq.joblib
│   │   │   ├── flask_app.py
│   │   │   ├── images
│   │   │   │   └── toc.png
│   │   │   ├── unittests.py
│   │   │   ├── utils.py
│   │   │   └── weaviate_server.py
│   │   └── labs
│   │       ├── 1
│   │       │   ├── C1M4_Ungraded_Lab_2.ipynb
│   │       │   └── utils.py
│   │       └── 2
│   │           ├── C1M4_Ungraded_Lab_1.ipynb
│   │           ├── images
│   │           │   ├── temperature.png
│   │           │   ├── toc.png
│   │           │   ├── top_k.png
│   │           │   └── top_p.png
│   │           └── utils.py
│   ├── notes
│   │   └── lecture_notes.md
│   ├── quiz
│   │   ├── 1.png
│   │   ├── 2.png
│   │   └── 3.png
│   └── slides
│       └── RAG_M4.pdf
├── module-5
│   ├── notebooks
│   │   ├── assignment
│   │   │   ├── C1M5_Assignment.ipynb
│   │   │   ├── dataset
│   │   │   │   ├── clothes.csv
│   │   │   │   ├── clothes_json.joblib
│   │   │   │   ├── clothing_ft_format.csv
│   │   │   │   └── faq.joblib
│   │   │   ├── flask_app.py
│   │   │   ├── images
│   │   │   │   ├── create_model_1.png
│   │   │   │   ├── create_model_2.png
│   │   │   │   ├── settings_1.png
│   │   │   │   └── toc.png
│   │   │   ├── unittesting.py
│   │   │   ├── utils.py
│   │   │   └── weaviate_server.py
│   │   └── labs
│   │       └── 1
│   │           ├── C1M5_Ungraded_Lab_1.ipynb
│   │           ├── faq.joblib
│   │           ├── flask_app.py
│   │           ├── images
│   │           │   ├── toc.png
│   │           │   ├── ui_1.png
│   │           │   ├── ui_2.png
│   │           │   ├── ui_3.png
│   │           │   └── ui_error.png
│   │           ├── utils.py
│   │           └── weaviate_server.py
│   ├── notes
│   │   └── lecture_notes.md
│   ├── quiz
│   │   ├── 1.png
│   │   ├── 2.png
│   │   └── 3.png
│   └── slides
│       └── RAG_M5.pdf
└── READMe.md
```