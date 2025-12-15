import streamlit as st
import pandas as pd

# --- Konfiguracja Strony ---
st.set_page_config(
    page_title="Prosty Magazyn Towarów",
    layout="wide",
    initial_sidebar_state="expanded"
)

## 📌 Inicjalizacja Magazynu (Sesja Streamlit)
# Używamy st.session_state do utrzymywania listy towarów
# po ponownym uruchomieniu skryptu (np. po interakcji użytkownika).
if 'magazyn' not in st.session_state:
    st.session_state['magazyn'] = [] # Pusta lista na towary

# --- Funkcje Magazynowe ---

def dodaj_towar(nazwa, ilosc, cena):
    """Dodaje nowy towar do listy w st.session_state."""
    if nazwa and ilosc is not None and cena is not None:
        try:
            # Konwersja danych i sprawdzenie, czy są poprawne
            ilosc_int = int(ilosc)
            cena_float = float(cena)
            
            if ilosc_int <= 0 or cena_float < 0:
                st.error("Ilość musi być większa od zera, a cena nieujemna.")
                return

            nowy_towar = {
                "Nazwa": nazwa,
                "Ilość": ilosc_int,
                "Cena Jednostkowa (PLN)": cena_float,
                "Wartość Całkowita (PLN)": ilosc_int * cena_float
            }
            st.session_state.magazyn.append(nowy_towar)
            st.success(f"Dodano towar: **{nazwa}** w ilości **{ilosc_int}**.")
            
            # Wyczyść pola formularza po udanym dodaniu
            st.session_state.nazwa_input = ""
            st.session_state.ilosc_input = None
            st.session_state.cena_input = None
            
        except ValueError:
            st.error("Ilość musi być liczbą całkowitą, a cena liczbą zmiennoprzecinkową.")
    else:
        st.warning("Wypełnij wszystkie pola, aby dodać towar.")


# --- Interfejs Użytkownika (Streamlit) ---

st.title("🛒 Prosty Magazyn Towarów (Streamlit/Python)")
st.markdown("Aplikacja przechowuje towary w pamięci sesji, bez zapisu do plików.")

# --- Sekcja Dodawania Towaru ---

st.header("➕ Dodaj Nowy Towar")

# Używamy formularza (st.form) do grupowania elementów
with st.form("dodawanie_towaru_form"):
    col1, col2, col3 = st.columns(3)
    
    with col1:
        nazwa = st.text_input("Nazwa Towaru", key="nazwa_input")
    
    with col2:
        # st.number_input pozwala na proste wprowadzenie liczby
        ilosc = st.number_input(
            "Ilość", 
            min_value=1, 
            step=1, 
            format="%d",
            key="ilosc_input"
        )
    
    with col3:
        cena = st.number_input(
            "Cena Jednostkowa (PLN)", 
            min_value=0.01, 
            step=0.01, 
            format="%.2f",
            key="cena_input"
        )
    
    # Przycisk do zatwierdzenia formularza
    submitted = st.form_submit_button("Dodaj do Magazynu")
    
    if submitted:
        # Po zatwierdzeniu wywołujemy funkcję dodającą towar
        dodaj_towar(nazwa, ilosc, cena)

st.markdown("---")

# --- Sekcja Wyświetlania Magazynu ---

st.header("📋 Stan Magazynu")

if st.session_state.magazyn:
    # Konwersja listy słowników na DataFrame dla ładniejszego wyświetlania
    df = pd.DataFrame(st.session_state.magazyn)
    
    # Wyświetlenie tabeli
    st.dataframe(df, use_container_width=True)
    
    # Dodatkowe podsumowanie
    suma_wartosci = df["Wartość Całkowita (PLN)"].sum()
    st.metric(
        label="Całkowita Wartość Magazynu", 
        value=f"{suma_wartosci:,.2f} PLN"
    )
    
    # Przycisk do czyszczenia magazynu (opcjonalnie)
    if st.button("Wyczyść Cały Magazyn"):
        st.session_state.magazyn = [] # Resetuje listę
        st.experimental_rerun() # Odświeża aplikację
else:
    st.info("Magazyn jest pusty. Dodaj pierwszy towar powyżej!")
