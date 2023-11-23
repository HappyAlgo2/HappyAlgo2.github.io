{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "48bc0a81-cdd7-40fb-8efd-e537ed1b5587",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<style>.container{width:100% !important;}</style>"
      ],
      "text/plain": [
       "<IPython.core.display.HTML object>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "from pykrx import stock\n",
    "from pykrx import bond\n",
    "import FinanceDataReader as fdr\n",
    "import yfinance as yf\n",
    "import time\n",
    "\n",
    "from IPython.display import Image\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "\n",
    "import warnings\n",
    "warnings.filterwarnings(action='ignore')\n",
    "sns.set()\n",
    "\n",
    "\n",
    "import talib.abstract as ta\n",
    "from talib import MA_Type\n",
    "\n",
    "\n",
    "import requests \n",
    "import json\n",
    "from bs4 import BeautifulSoup \n",
    "import datetime \n",
    "from pandas.io.json import json_normalize\n",
    "from sklearn.preprocessing import StandardScaler\n",
    "from matplotlib.animation import FuncAnimation\n",
    "\n",
    "\n",
    "#-------------------- 차트 관련 속성 (한글처리, 그리드) -----------\n",
    "#plt.rc('font', family='NanumGothicOTF') # For MacOS\n",
    "plt.rcParams['font.family']= 'Malgun Gothic'\n",
    "plt.rcParams['axes.unicode_minus'] = False\n",
    "\n",
    "\n",
    "#-------------------- 주피터 , 출력결과 넓이 늘리기 ---------------\n",
    "from IPython.core.display import display, HTML\n",
    "display(HTML(\"<style>.container{width:100% !important;}</style>\"))\n",
    "pd.set_option('display.max_rows', 100)\n",
    "pd.set_option('display.max_columns', 100)\n",
    "pd.set_option('max_colwidth', None)\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "467b70f8-dce5-41b7-af59-8aaf6cb2cf58",
   "metadata": {},
   "outputs": [],
   "source": [
    "from pykrx import stock\n",
    "# from pykrx import bond"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "49b73b1e-eb84-495a-a746-489e308482e6",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "코스피 200         시가      고가      저가      종가        거래량           거래대금  \\\n",
      "날짜                                                                     \n",
      "1990-01-03    0.00    0.00    0.00  100.00    4024570              0   \n",
      "1990-01-04    0.00    0.00    0.00  102.00   10004880              0   \n",
      "1990-01-05    0.00    0.00    0.00  100.38   10657720              0   \n",
      "1990-01-06    0.00    0.00    0.00  100.02    5440870              0   \n",
      "1990-01-08    0.00    0.00    0.00  100.51    7805900              0   \n",
      "...            ...     ...     ...     ...        ...            ...   \n",
      "2023-10-25  319.26  319.59  316.30  316.49   91218212  5990647874872   \n",
      "2023-10-26  311.36  312.37  307.75  307.75  110774427  7147843599760   \n",
      "2023-10-27  309.68  310.43  307.24  308.53   92934386  5978754499550   \n",
      "2023-10-30  307.01  310.12  306.89  308.95   95847666  4917393348916   \n",
      "2023-10-31  310.49  311.58  305.14  305.56  121721241  6285058452388   \n",
      "\n",
      "코스피 200               상장시가총액  \n",
      "날짜                            \n",
      "1990-01-03                 0  \n",
      "1990-01-04                 0  \n",
      "1990-01-05                 0  \n",
      "1990-01-06                 0  \n",
      "1990-01-08                 0  \n",
      "...                      ...  \n",
      "2023-10-25  1638326343330425  \n",
      "2023-10-26  1594021634219925  \n",
      "2023-10-27  1597031432203450  \n",
      "2023-10-30  1601800798334580  \n",
      "2023-10-31  1580134815560475  \n",
      "\n",
      "[8768 rows x 7 columns]\n"
     ]
    }
   ],
   "source": [
    "df = stock.get_index_ohlcv_by_date(\"19760102\", \"20231031\", \"1028\")\n",
    "print(df)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "bf8f9bbb-6c58-40cf-963c-03a54b5b509e",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[]\n"
     ]
    }
   ],
   "source": [
    "# tickers = stock.get_index_portfolio_deposit_file(\"KQ150\")\n",
    "# print(tickers)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "581a2233-fb48-46ff-8c57-730eecb43f6d",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "HTTP Error 404: Not Found  - symbol \"KQ150\"not found or invalid periods\n"
     ]
    }
   ],
   "source": [
    "# df_kq150 = fdr.DataReader('KQ150','2020-01-01','2022-07-15')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "id": "2173d586-26f7-4589-bbd1-e797270cb4c4",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "코스닥 150          시가       고가       저가       종가       거래량           거래대금  \\\n",
      "날짜                                                                        \n",
      "2010-01-04     0.00     0.00     0.00  1000.00         0              0   \n",
      "2010-01-05     0.00     0.00     0.00  1005.86         0              0   \n",
      "2010-01-06     0.00     0.00     0.00  1020.16         0              0   \n",
      "2010-01-07     0.00     0.00     0.00  1010.69         0              0   \n",
      "2010-01-08     0.00     0.00     0.00  1017.34         0              0   \n",
      "...             ...      ...      ...      ...       ...            ...   \n",
      "2023-10-25  1251.63  1251.63  1208.37  1208.41  77715194  2431328571853   \n",
      "2023-10-26  1170.79  1185.09  1156.56  1160.79  69778933  2715254637081   \n",
      "2023-10-27  1175.67  1196.56  1156.03  1175.63  69418598  2479348097341   \n",
      "2023-10-30  1173.95  1197.93  1170.28  1189.29  48369139  1726472738492   \n",
      "2023-10-31  1188.45  1188.76  1144.50  1147.92  50799539  1959158683552   \n",
      "\n",
      "코스닥 150              상장시가총액  \n",
      "날짜                           \n",
      "2010-01-04                0  \n",
      "2010-01-05                0  \n",
      "2010-01-06                0  \n",
      "2010-01-07                0  \n",
      "2010-01-08                0  \n",
      "...                     ...  \n",
      "2023-10-25  192954253773567  \n",
      "2023-10-26  185235604286077  \n",
      "2023-10-27  187800419270280  \n",
      "2023-10-30  190373571128497  \n",
      "2023-10-31  183737159616416  \n",
      "\n",
      "[3412 rows x 7 columns]\n"
     ]
    }
   ],
   "source": [
    "df2 = stock.get_index_ohlcv_by_date(\"19760102\", \"20231031\", \"2203\")\n",
    "print(df2)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "5e958c4e-47c2-4a7f-8f50-1fc1082eefe7",
   "metadata": {},
   "source": [
    "1001 코스피\r\n",
    "1028 코스피 200\r\n",
    "1034 코스피 100\r\n",
    "1035 코스피 50\r\n",
    "1167 코스피 200 중소형주\r\n",
    "1182 코스피 200 초대형제외 지수\r\n",
    "1244 코스피200제외 코스피지수\r\n",
    "1150 코스피 200 커뮤니케이션서비스\r\n",
    "1151 코스피 200 건설\r\n",
    "1152 코스피 200 중공업\r\n",
    "1153 코스피 200 철강/소재\r\n",
    "1154 코스피 200 에너지/화학\r\n",
    "1155 코스피 200 정보기술\r\n",
    "1156 코스피 200 금융\r\n",
    "1157 코스피 200 생활소비재\r\n",
    "1158 코스피 200 경기소비재\r\n",
    "1159 코스피 200 산업재\r\n",
    "1160 코스피 200 헬스케어\r\n",
    "1005 음식료품\r\n",
    "1006 섬유의복\r\n",
    "1007 종이목재\r\n",
    "1008 화학\r\n",
    "1009 의약품\r\n",
    "1010 비금속광물\r\n",
    "1011 철강금속\r\n",
    "1012 기계\r\n",
    "1013 전기전자\r\n",
    "1014 의료정밀\r\n",
    "1015 운수장비\r\n",
    "1016 유통업\r\n",
    "1017 전기가스업\r\n",
    "1018 건설업\r\n",
    "1019 운수창고업\r\n",
    "1020 통신업\r\n",
    "1021 금융업\r\n",
    "1022 은행\r\n",
    "1024 증권\r\n",
    "1025 보험\r\n",
    "1026 서비스업\r\n",
    "1027 제조업\r\n",
    "1002 코스피 대형주\r\n",
    "1003 코스피 중형주\r\n",
    "1004 코스피 소형주\r\n",
    "1224 코스피 200 비중상한 30%\r\n",
    "1227 코스피 200 비중상한 25%\r\n",
    "1232 코스피 200 비중상한 20%\r\n",
    " "
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c0d6e5fa-ac29-4e91-9d89-61f1ab5951b3",
   "metadata": {},
   "source": [
    "2001 코스닥\n",
    "2203 코스닥 150\n",
    "2216 코스닥 150 정보기술\n",
    "2217 코스닥 150 헬스케어\n",
    "2218 코스닥 150 커뮤니케이션서비스\n",
    "2212 코스닥 150 소재\n",
    "2213 코스닥 150 산업재\n",
    "2214 코스닥 150 필수소비재\n",
    "2215 코스닥 150 자유소비재\n",
    "2012 기타서비스\n",
    "2015 코스닥 IT\n",
    "2024 제조\n",
    "2026 건설\n",
    "2027 유통\n",
    "2029 운송\n",
    "2031 금융\n",
    "2037 오락,문화\n",
    "2041 통신방송서비스\n",
    "2042 IT S/W & SVC\n",
    "2043 IT H/W\n",
    "2056 음식료·담배\n",
    "2058 섬유·의류\n",
    "2062 종이·목재\n",
    "2063 출판·매체복제\n",
    "2065 화학\n",
    "2066 제약\n",
    "2067 비금속\n",
    "2068 금속\n",
    "2070 기계·장비\n",
    "2072 일반전기전자\n",
    "2074 의료·정밀기기\n",
    "2075 운송장비·부품\n",
    "2077 기타 제조\n",
    "2151 통신서비스\n",
    "2152 방송서비스\n",
    "2153 인터넷\n",
    "2154 디지털컨텐츠\n",
    "2155 소프트웨어\n",
    "2156 컴퓨터서비스\n",
    "2157 통신장비\n",
    "2158 정보기기\n",
    "2159 반도체\n",
    "2160 IT부품\n",
    "2002 코스닥 대형주\n",
    "2003 코스닥 중형주\n",
    "2004 코스닥 소형주\n",
    "2181 코스닥 우량기업부\n",
    "2182 코스닥 벤처기업부\n",
    "2183 코스닥 중견기업부\n",
    "2184 코스닥 기술성장기업부"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "dd723620-7d0b-49e3-98c9-0ec255dc8117",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "20 ['035760', '025770', '040300', '122450', '451760', '036800', '048550', '058400', '066790', '036630', '039340', '408900', '039290', '033830', '066410', '065530', '115310', '039420', '052600', '131100']\n"
     ]
    }
   ],
   "source": [
    "pdf = stock.get_index_portfolio_deposit_file(\"2041\")\n",
    "print(len(pdf), pdf)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7ca61bb3-7e8e-41be-95de-0d0ab1ab7eee",
   "metadata": {},
   "source": [
    "# 인덱스 조회 API"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "id": "995a54da-992a-45d8-8585-98d8b374d57c",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "46 ['1001', '1002', '1003', '1004', '1005', '1006', '1007', '1008', '1009', '1010', '1011', '1012', '1013', '1014', '1015', '1016', '1017', '1018', '1019', '1020', '1021', '1024', '1025', '1026', '1027', '1028', '1034', '1035', '1150', '1151', '1152', '1153', '1154', '1155', '1156', '1157', '1158', '1159', '1160', '1167', '1182', '1224', '1227', '1232', '1244', '1894']\n"
     ]
    }
   ],
   "source": [
    "# 코스피 인덱스\n",
    "tickers = stock.get_index_ticker_list()\n",
    "print(len(tickers), tickers)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "id": "26afc588-8969-45fc-a0bf-8eb7e4d7b96f",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "51 ['2001', '2002', '2003', '2004', '2012', '2015', '2024', '2026', '2027', '2029', '2031', '2037', '2041', '2042', '2043', '2056', '2058', '2062', '2063', '2065', '2066', '2067', '2068', '2070', '2072', '2074', '2075', '2077', '2151', '2152', '2153', '2154', '2155', '2156', '2157', '2158', '2159', '2160', '2181', '2182', '2183', '2184', '2189', '2203', '2212', '2213', '2214', '2215', '2216', '2217', '2218']\n"
     ]
    }
   ],
   "source": [
    "# 코스닥 인덱스\n",
    "tickers = stock.get_index_ticker_list(market='KOSDAQ')\n",
    "print(len(tickers), tickers)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "ad0fa10a-16e4-4b3f-913c-c4795a42cdd3",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "1001 코스피\n",
      "1002 코스피 대형주\n",
      "1003 코스피 중형주\n",
      "1004 코스피 소형주\n",
      "1005 음식료품\n",
      "1006 섬유의복\n",
      "1007 종이목재\n",
      "1008 화학\n",
      "1009 의약품\n",
      "1010 비금속광물\n",
      "1011 철강금속\n",
      "1012 기계\n",
      "1013 전기전자\n",
      "1014 의료정밀\n",
      "1015 운수장비\n",
      "1016 유통업\n",
      "1017 전기가스업\n",
      "1018 건설업\n",
      "1019 운수창고업\n",
      "1020 통신업\n",
      "1021 금융업\n",
      "1024 증권\n",
      "1025 보험\n",
      "1026 서비스업\n",
      "1027 제조업\n",
      "1028 코스피 200\n",
      "1034 코스피 100\n",
      "1035 코스피 50\n",
      "1150 코스피 200 커뮤니케이션서비스\n",
      "1151 코스피 200 건설\n",
      "1152 코스피 200 중공업\n",
      "1153 코스피 200 철강/소재\n",
      "1154 코스피 200 에너지/화학\n",
      "1155 코스피 200 정보기술\n",
      "1156 코스피 200 금융\n",
      "1157 코스피 200 생활소비재\n",
      "1158 코스피 200 경기소비재\n",
      "1159 코스피 200 산업재\n",
      "1160 코스피 200 헬스케어\n",
      "1167 코스피 200 중소형주\n",
      "1182 코스피 200 초대형제외 지수\n",
      "1224 코스피 200 비중상한 30%\n",
      "1227 코스피 200 비중상한 25%\n",
      "1232 코스피 200 비중상한 20%\n",
      "1244 코스피200제외 코스피지수\n",
      "1894 코스피 200 TOP 10\n"
     ]
    }
   ],
   "source": [
    "for ticker in stock.get_index_ticker_list():\n",
    "    print(ticker, stock.get_index_ticker_name(ticker))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "id": "a40d9b48-e4fd-4d5e-9096-a1e0113406ac",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "2001 코스닥\n",
      "2002 코스닥 대형주\n",
      "2003 코스닥 중형주\n",
      "2004 코스닥 소형주\n",
      "2012 기타서비스\n",
      "2015 코스닥 IT\n",
      "2024 제조업\n",
      "2026 건설\n",
      "2027 유통\n",
      "2029 운송\n",
      "2031 금융\n",
      "2037 오락·문화\n",
      "2041 통신방송서비스\n",
      "2042 IT S/W & SVC\n",
      "2043 IT H/W\n",
      "2056 음식료·담배\n",
      "2058 섬유·의류\n",
      "2062 종이·목재\n",
      "2063 출판·매체복제\n",
      "2065 화학\n",
      "2066 제약\n",
      "2067 비금속\n",
      "2068 금속\n",
      "2070 기계·장비\n",
      "2072 일반전기전자\n",
      "2074 의료·정밀기기\n",
      "2075 운송장비·부품\n",
      "2077 기타제조\n",
      "2151 통신서비스\n",
      "2152 방송서비스\n",
      "2153 인터넷\n",
      "2154 디지털컨텐츠\n",
      "2155 소프트웨어\n",
      "2156 컴퓨터서비스\n",
      "2157 통신장비\n",
      "2158 정보기기\n",
      "2159 반도체\n",
      "2160 IT부품\n",
      "2181 코스닥 우량기업부\n",
      "2182 코스닥 벤처기업부\n",
      "2183 코스닥 중견기업부\n",
      "2184 코스닥 기술성장기업부\n",
      "2189 코스닥 글로벌\n",
      "2203 코스닥 150\n",
      "2212 코스닥 150 소재\n",
      "2213 코스닥 150 산업재\n",
      "2214 코스닥 150 필수소비재\n",
      "2215 코스닥 150 자유소비재\n",
      "2216 코스닥 150 정보기술\n",
      "2217 코스닥 150 헬스케어\n",
      "2218 코스닥 150 커뮤니케이션서비스\n"
     ]
    }
   ],
   "source": [
    "for ticker in stock.get_index_ticker_list(market='KOSDAQ'):\n",
    "    print(ticker, stock.get_index_ticker_name(ticker))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "id": "fce8d4ec-b826-463d-b176-1ed9805148cd",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "               종가  등락률  PER  선행PER  PBR  배당수익률\n",
      "날짜                                            \n",
      "1976-01-05  87.77  0.0  0.0    0.0  0.0    0.0\n",
      "1976-01-06  89.94  0.0  0.0    0.0  0.0    0.0\n",
      "1976-01-07  89.84  0.0  0.0    0.0  0.0    0.0\n",
      "1976-01-08  88.93  0.0  0.0    0.0  0.0    0.0\n",
      "1976-01-09  89.80  0.0  0.0    0.0  0.0    0.0\n"
     ]
    }
   ],
   "source": [
    "df = stock.get_index_fundamental(\"19760102\", \"20231031\", \"1001\")\n",
    "print(df.head())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "da578849-2cd3-4eb4-9346-92555275ca40",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "해당티커 :  5042 지수명 :  KRX 100\n",
      "해당티커 :  5043 지수명 :  KRX 자동차\n",
      "해당티커 :  5044 지수명 :  KRX 반도체\n",
      "해당티커 :  5045 지수명 :  KRX 헬스케어\n",
      "해당티커 :  5046 지수명 :  KRX 은행\n",
      "해당티커 :  5048 지수명 :  KRX 에너지화학\n",
      "해당티커 :  5049 지수명 :  KRX 철강\n",
      "해당티커 :  5051 지수명 :  KRX 방송통신\n",
      "해당티커 :  5052 지수명 :  KRX 건설\n",
      "해당티커 :  5054 지수명 :  KRX 증권\n",
      "해당티커 :  5055 지수명 :  KRX 기계장비\n",
      "해당티커 :  5056 지수명 :  KRX 보험\n",
      "해당티커 :  5057 지수명 :  KRX 운송\n",
      "해당티커 :  5061 지수명 :  KRX 경기소비재\n",
      "해당티커 :  5062 지수명 :  KRX 필수소비재\n",
      "해당티커 :  5063 지수명 :  KRX 미디어&엔터테인먼트\n",
      "해당티커 :  5064 지수명 :  KRX 정보기술\n",
      "해당티커 :  5065 지수명 :  KRX 유틸리티\n",
      "해당티커 :  5300 지수명 :  KRX 300\n",
      "해당티커 :  5351 지수명 :  KRX 300 정보기술\n",
      "해당티커 :  5352 지수명 :  KRX 300 금융\n",
      "해당티커 :  5353 지수명 :  KRX 300 자유소비재\n",
      "해당티커 :  5354 지수명 :  KRX 300 산업재\n",
      "해당티커 :  5355 지수명 :  KRX 300 헬스케어\n",
      "해당티커 :  5356 지수명 :  KRX 300 커뮤니케이션서비스\n",
      "해당티커 :  5357 지수명 :  KRX 300 소재\n",
      "해당티커 :  5358 지수명 :  KRX 300 필수소비재\n",
      "해당티커 :  5600 지수명 :  KTOP 30\n"
     ]
    }
   ],
   "source": [
    "from openpyxl import Workbook\n",
    "\n",
    "for ticker in stock.get_index_ticker_list(market=\"KRX\"):   # () : 코스피, (market=\"KOSDAQ\") : 코스닥\n",
    "    ticker_name = stock.get_index_ticker_name(ticker)\n",
    "    \n",
    "    # 티커 이름에서 특수 문자와 공백을 제거하거나 대체\n",
    "    filename = ticker_name.replace(' ', '_').replace('/', '_').replace('-', '_')\n",
    "    \n",
    "    print('해당티커 : ', ticker, '지수명 : ', ticker_name)\n",
    "    \n",
    "    df = stock.get_index_fundamental(\"19760102\", \"20231031\", ticker)\n",
    "    df.to_excel(f'{filename}_fundamental.xlsx')\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "623a47ea-bca4-4b94-b915-db027f8cd90e",
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "ea34f4b0-921d-492e-98f3-d7d6c52888ce",
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "4111463e-a856-4287-b8de-56ff04429178",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.8.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
