
import streamlit as st
import streamlit.components.v1 as components
import pydeck as pdk

import numpy as np
import pandas as pd 

import altair as alt

alt.renderers.set_embed_options(theme='dark')

from datetime import datetime

st.set_page_config(page_title='Fire Incidends, Toronto',
                    page_icon="🔥",
                    layout='wide')


st.title("Fire Incidends in Toronto, Canada 2011-2019 🔥")
def load_data():
    df_fire = pd.read_csv("Fire-Incidents.csv")
    return df_fire
#Map
df_fire = load_data()
data= df_fire[['Latitude','Longitude']].rename(columns={'Latitude': "lat",'Longitude':"lon"})
layer = pdk.Layer(
        "ScatterplotLayer",
        data= df_fire[['Latitude','Longitude']],
        pickable=True,
        opacity=0.4,
        stroked=True,
        filled=True,
        radius_scale=10,
        radius_min_pixels=1,
        radius_max_pixels=100,
        line_width_min_pixels=1,
        get_position=['Latitude', 'Longitude'],
        get_color=[255, 140, 0],
        get_line_color=[0, 0, 0])

st.map(data)
view_state = pdk.ViewState(latitude=df_fire['Latitude'].mean(), longitude=df_fire['Longitude'].mean(), zoom=12, pitch=50)
r = pdk.Deck(layers=[layer], 
initial_view_state=view_state)
