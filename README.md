Mapa de Calor

#importar bibliotecas
import folium
import pandas as pd
df = pd.read_csv('C:/Users/Felipe/Desktop/new/cat_acidentes.csv', sep = ';')
df


#Criando o mapa de calor
from folium.plugins import HeatMap
mapa = folium.Map(location=[-30.1, -51.15], zoom_start=11)
coordenadas = list(zip(df.latitude, df.longitude))
mapa_calor = HeatMap(coordenadas, radius=9, blur=10)
mapa.add_child(mapa_calor)

#exibir o mapa
mapa

#Limpando os "NANS"
df = df.dropna(subset=['latitude', 'longitude'], how='any')
df

#Criando mapa numerico
from folium.plugins import MarkerCluster
mapa = folium.Map(location=[-30.1, -51.15], zoom_start=11)
mapa_cluster = MarkerCluster(coordenadas)
mapa.add_child(mapa_cluster)

mapa

#criando grafico 
df['data'] = pd.to_datetime(df['data'], errors='coerce')
df_ano = df['data'].dt.year.value_counts()
df_ano

df_ano = df_ano.drop(2202)
df_ano

import matplotlib.pyplot as plt
plt.bar(df_ano.index, df_ano.values)
plt.show()

df_ano/df_ano.max()

gradiente = df_ano/df_ano.max()
cores = plt.cm.Blues(gradiente)

plt.bar(df_ano.index, df_ano.values, color = cores)
plt.show()
