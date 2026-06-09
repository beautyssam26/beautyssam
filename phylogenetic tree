import streamlit as st
import pandas as pd
import plotly.express as px

# 1. 페이지 기본 설정 및 타이틀
st.set_page_config(page_title="생물 계통분류 탐구 활동", layout="wide", initial_sidebar_state="expanded")

st.title("🧬 생물 계통분류체계 시각화 탐구")
st.markdown("""
학생 여러분! 이 앱을 통해 **역-계-문-강-목-과-속-종**으로 이어지는 생물의 분류 계층을 시각적으로 탐구할 수 있습니다.
* **원리 기억하기**: 상위 분류군(역, 계)으로 갈수록 포함되는 생물종의 범위가 **넓어지고**, 하위 분류군(속, 종)으로 갈수록 서로 공유하는 **공통점이 많아집니다.**
---
""")

# 2. 수업용 샘플 데이터셋 구성 (고등학교 생명과학 교과서에 자주 등장하는 대표 생물)
@st.cache_data
def load_taxonomy_data():
    data = [
        # 진핵생물역 - 동물계
        {"역": "진핵생물역", "계": "동물계", "문": "척삭동물문", "강": "포유강", "목": "영장목", "과": "사람과", "속": "사람속", "종": "사람 (Homo sapiens)"},
        {"역": "진핵생물역", "계": "동물계", "문": "척삭동물문", "강": "포유강", "목": "식육목", "과": "고양이과", "속": "표범속", "종": "호랑이 (Panthera tigris)"},
        {"역": "진핵생물역", "계": "동물계", "문": "척삭동물문", "강": "포유강", "목": "식육목", "과": "개과", "속": "개속", "종": "개 (Canis lupus)"},
        {"역": "진핵생물역", "계": "동물계", "문": "척삭동물문", "강": "포유강", "목": "식육목", "과": "고양이과", "속": "고양이속", "종": "고양이 (Felis catus)"},
        # 진핵생물역 - 식물계
        {"역": "진핵생물역", "계": "식물계", "문": "관다발식물문", "강": "소나무강", "목": "소나무목", "과": "소나무과", "속": "소나무속", "종": "소나무 (Pinus densiflora)"},
        {"역": "진핵생물역", "계": "식물계", "문": "피자식물문", "강": "쌍떡잎식물강", "목": "장미목", "과": "장미과", "속": "장미속", "종": "장미 (Rosa hybrida)"},
        # 진핵생물역 - 균계
        {"역": "진핵생물역", "계": "균계", "문": "담자균문", "강": "주름버섯강", "목": "주름버섯목", "과": "광대버섯과", "속": "광대버섯속", "종": "독우산광대버섯 (Amanita virosa)"},
        # 세균역
        {"역": "세균역", "계": "진정세균계", "문": "의사문", "강": "감마변형진정세균강", "목": "장내세균목", "과": "장내세균과", "속": "대장균속", "종": "대장균 (Escherichia coli)"},
        # 고세균역
        {"역": "고세균역", "계": "고세균계", "문": "유리고세균문", "강": "메탄생성균강", "목": "메탄바실루스목", "과": "메탄바실루스과", "속": "메탄바실루스속", "종": "메탄생성균 (Methanobacterium)"}
    ]
    return pd.DataFrame(data)

df = load_taxonomy_data()

# 3. 탭(Tab) 구성을 통해 두 가지 핵심 기능 제공
tab1, tab2 = st.tabs(["📊 전체 계통분류 구조 탐색", "🔍 특정 생물종 역추적"])

# ----------------------------------------------------
# Tab 1: 전체 계통분류 구조 탐색 (Sunburst Chart)
# ----------------------------------------------------
with tab1:
    st.header("계층 구조 시각화 (Sunburst Chart)")
    st.subheader("💡 학습 가이드")
    st.info("""
    * 차트의 안쪽 원(역, 계)에서 바깥쪽 원(속, 종)으로 클릭해 들어가 보세요!
    * **안쪽 단계**: 넓은 범위의 생물이 모여 있어, 서로 형태나 생리적 공통점은 적지만 거대한 생물군을 이룹니다.
    * **바깥쪽 단계**: 범위가 극도로 좁아지며, 최종 '종'에 이르면 교배하여 생식 능력이 있는 자손을 낳을 수 있을 만큼 공통점이 많아집니다.
    """)
    
    # 계층 구조 정의
    levels = ["역", "계", "문", "강", "목", "과", "속", "종"]
    
    # Plotly Sunburst 차트 생성
    # 모든 종의 크기(value)를 동일하게 1로 세팅하여 시각적 균형을 맞춤
    df['count'] = 1
    fig = px.sunburst(
        df, 
        path=levels, 
        values='count',
        color='계', # '계' 수준에서 색상을 다채롭게 분기
        title="계통분류학적 계층 구조 (클릭하면 확대/축소 가능)",
        color_discrete_sequence=px.colors.qualitative.Pastel
    )
    
    # 글자 크기 및 차트 레이아웃 조정
    fig.update_traces(textinfo="label")
    fig.update_layout(width=800, height=800)
    
    # 스트림릿 화면에 차트 출력
    st.plotly_chart(fig, use_container_width=True)


# ----------------------------------------------------
# Tab 2: 특정 생물종 역추적 (Reverse Tracing)
# ----------------------------------------------------
with tab2:
    st.header("생물종 검색 및 분류 단계 역추적")
    st.write("관심 있는 생물을 선택하면, 최하위 분류군인 **'종'**에서부터 최상위 분류군인 **'역'**까지 어떻게 거슬러 올라가는지 보여줍니다.")
    
    # 셀렉트 박스를 통한 생물종 선택
    selected_species = st.selectbox("탐색할 생물종을 선택하세요:", df["종"].unique())
    
    # 선택된 생물의 데이터 추출
    species_info = df[df["종"] == selected_species].iloc[0]
    
    st.markdown(f"### 📍 **{selected_species}**의 상위 분류 단계")
    
    # Graphviz를 활용한 깔끔한 트리 흐름도 시각화
    # 상위(역)에서 하위(종)로 내려오는 흐름을 시각적으로 명확하게 정렬
    dot_code = f"""
    digraph G {{
        rankdir=LR;
        node [shape=box, style="filled,rounded", color="#4EA8DE", fillcolor="#E2EAFC", fontname="NanumGothic, Arial", fontsize=12];
        edge [color="#3A0CA3", width=2];
        
        "역: {species_info['역']}" -> "계: {species_info['계']}" -> "문: {species_info['문']}" -> "강: {species_info['강']}" -> "목: {species_info['목']}" -> "과: {species_info['과']}" -> "속: {species_info['속']}" -> "종: {species_info['종']}";
    }}
    """
    st.graphviz_chart(dot_code)
    
    # 표 형태로 깔끔하게 다시 정리해 주기
    st.subheader("📋 분류 단계별 명칭 요약")
    summary_df = pd.DataFrame({
        "분류 단계": ["역 (Domain)", "계 (Kingdom)", "문 (Phylum)", "강 (Class)", "목 (Order)", "과 (Family)", "속 (Genus)", "종 (Species)"],
        "해당 생물의 분류군 명칭": [species_info['역'], species_info['계'], species_info['문'], species_info['강'], species_info['목'], species_info['과'], species_info['속'], species_info['종']]
    })
    st.table(summary_df)
    
    # 교사를 위한 개념 심화 퀴즈/피드백 섹션
    st.markdown("---")
    st.subheader("💡 생각해보기를 위한 질문 (토론 팁)")
    
    # 간단한 확장형 컴포넌트로 수업 중 질문 유도
    with st.expander("Q1. 호랑이와 고양이는 어느 단계까지 같은 분류군에 속해 있을까요?"):
        st.write("""
        * **정답**: **'속'** 직전 단계인 **'과(고양이과)'**까지 동일합니다!
        * 호랑이는 *표범속*, 고양이는 *고양이속*으로 갈라지므로, 과 단계까지의 공통적 특성(유연한 몸, 발톱 숨기기 등)을 공유하지만 속 단계부터는 체구와 세부 생태가 확연히 달라집니다.
        """)
        
    with st.expander("Q2. 대장균과 사람은 왜 '역' 단계부터 갈라질까요?"):
        st.write("""
        * **정답**: 세포 구조 자체가 근본적으로 다르기 때문입니다.
        * 대장균은 핵이 없는 **세균역(원핵생물)**에 속하고, 사람은 핵과 막성 세포소기관을 가진 세포로 이루어진 **진핵생물역**에 속합니다. 이는 생물학에서 가장 거대한 분기점 중 하나입니다.
        """)
