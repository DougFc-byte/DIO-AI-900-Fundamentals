#Explorar um Azure AI Search index (UI)
O desafio proposto foi imaginar que trabalho para uma cadeia nacional de café, Fourth Coffee, e que tenho que ajudar a construir uma solução de mineração de conhecimento que pode facilmente buscar por insights na experiência do consumidor e eu decidi construir um Index do Azure AI Search usando os dados disponíveis.
1. Neste lab, fiz o seguinte: 1. Criar recursos do Azure; 2. Extrair dados de uma fonte; 3. Enriquecer os dados com as habilidades da IA; 4. Usar o Indexer do Azure; 5. Dar query no index; 6. Revisar os resultados salvos num Knowledge Store.
1. Primeiramente, criei três recursos, do Azure AI Search, Storage Account e AI Services seguindo a documentação.
1. Depois, fiz o upload da base de dados zipada dos reviews para o Azure Storage.
2. Depois, usei o Azure Index para extrair insights através do import data wizard e conectando os dados.
3. Enriqueci os dados através do AI Services do Azure e de acordo com a documentação;
4. Depois, fiz queries ao index, como por exemplo:
{
    "search": "*",
    "count": true
}
![image](https://github.com/user-attachments/assets/a3266208-4709-4da6-8653-c4d6f4754615)
5. Filtrando por local:
{
    "search": "locations:'Chicago'",
    "count": true
}
![image](https://github.com/user-attachments/assets/d150bc07-cff4-4e70-b237-1870fada45ce)
6. Filtrando por sentimento negativo:
{
    "search": "sentiment:'negative'",
    "count": true
}

![image](https://github.com/user-attachments/assets/fbc64535-8ece-4c95-a177-c55ff950e5f7)

7. Depois vi o knowledge store container com os dados enriquecidos pela IA na forma de projeções e tabelas. 
![image](https://github.com/user-attachments/assets/dfd2d14d-5cc3-41c6-8914-8e756181fe0f)
![image](https://github.com/user-attachments/assets/1736b9f0-f411-4438-8891-a4822079f218)
![image](https://github.com/user-attachments/assets/1c927720-44ee-4984-b378-1a6f8d7d93db)

8. A conclusão é que é um serviço muito interessante, principalmente quando pensamos que podemos facilmentente indexar, enriquecer e extrair dados para usar em outras soluções como o Power BI.
