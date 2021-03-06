# Create your first app
- 스크립트 파일 생성
- 라이브러리 로딩
- 스크립트 작성
- 실행
- 결과 확인 및 수정 등

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")


```

![image](https://user-images.githubusercontent.com/102650331/169686010-df8b217b-6d42-466c-9a33-79ac26177094.png)

![image](https://user-images.githubusercontent.com/102650331/169686407-a148a995-0832-446b-bb8d-7814a5c02c62.png)

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text('Loading data...done!')

```

![image](https://user-images.githubusercontent.com/102650331/169686499-a236d278-94df-47e8-b9f6-28eba331e02a.png)

# Effortless caching

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

```

![image](https://user-images.githubusercontent.com/102650331/169686764-a0397e67-f87f-427b-bf7f-5f7a1ac88d06.png)

## How's it work?
- @st.cache 동작 방법 설명
- Streamlit 함수 호출할 때 다음 3가지를 확인한다.
    - The actual bytecode that makes up the body of the function
    - 함수의 본문의 변경
    - Code, variables, and files that the function depends on
    - 함수가 참조하는 코드, 파일, 변수의 변경
    - The input parameters that you called the function with
    - 함수호출 변수 값 변경
- 최초 함수 호출시 실행, 실행결과 local cache 저장
- 3가지 변경 사항이 없으면 local cache 에서 결과를 읽어 반환
- 현재 작업 디렉토리 내의 변경 사항만 확인

# Inspect the raw data

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

st.subheader('Raw data')
st.write(data)

```

![image](https://user-images.githubusercontent.com/102650331/169688427-15ff16fd-5117-480b-8bd3-111bbbb90c1a.png)

# Draw a histogram

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

st.subheader('Raw data')
st.write(data)

st.subheader('Number of pickups by hour')

# 시간 단위 히스토그램 생성
hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

st.bar_chart(hist_values)

```
![image](https://user-images.githubusercontent.com/102650331/169688650-0ae933df-08dc-4620-aea5-1170e18a0ed1.png)


# Plot data on a map

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

st.subheader('Raw data')
st.write(data)

st.subheader('Number of pickups by hour')

# 시간 단위 히스토그램 생성
hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

st.bar_chart(hist_values)

st.subheader('Map of all pickups')
st.map(data)


```
![image](https://user-images.githubusercontent.com/102650331/169688838-8356f7af-6aa4-4b51-becf-8b24b85df4ea.png)

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

st.subheader('Raw data')
st.write(data)

st.subheader('Number of pickups by hour')

# 시간 단위 히스토그램 생성
hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

st.bar_chart(hist_values)

# st.subheader('Map of all pickups')
# st.map(data)

hour_to_filter = 17
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]
st.subheader(f'Map of all pickups at {hour_to_filter}:00')
st.map(filtered_data)


```

![image](https://user-images.githubusercontent.com/102650331/169688912-57028685-70d6-474f-b658-ed63f04eb0b7.png)


```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

st.subheader('Raw data')
st.write(data)

st.subheader('Number of pickups by hour')

# 시간 단위 히스토그램 생성
hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

st.bar_chart(hist_values)

# st.subheader('Map of all pickups')
# st.map(data)

# hour_to_filter = 17

hour_to_filter = st.slider('hour', 0, 23, 17)  # min: 0h, max: 23h, default: 17h
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]
st.subheader(f'Map of all pickups at {hour_to_filter}:00')
st.map(filtered_data)


```

![image](https://user-images.githubusercontent.com/102650331/169689026-133c9346-c080-46f8-be7e-f53a0f0fbe25.png)

# Use a button to toggle data

```python
# uber_pickups.py

import streamlit as st
import pandas as pd
import numpy as np

st.title("Uber pickups in NYC")

DATE_COLUMN = "date/time"
DATA_URL = ("https://s3-us-west-2.amazonaws.com/"
                "streamlit-demo-data/uber-raw-data-sep14.csv.gz")

@st.cache
def load_data(nrows):

    """
    @nrows: 데이터프레임에서 추출한 레코드 숫자
    """

    data = pd.read_csv(DATA_URL, nrows=nrows)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis="columns", inplace=True)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done! (using st.cache)")

# st.subheader('Raw data')
# st.write(data)

if st.checkbox('Show raw data'):
    st.subheader('Raw data')
    st.write(data)

st.subheader('Number of pickups by hour')

# 시간 단위 히스토그램 생성
hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

st.bar_chart(hist_values)

# st.subheader('Map of all pickups')
# st.map(data)

# hour_to_filter = 17

hour_to_filter = st.slider('hour', 0, 23, 17)  # min: 0h, max: 23h, default: 17h
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]
st.subheader(f'Map of all pickups at {hour_to_filter}:00')
st.map(filtered_data)


```

![image](https://user-images.githubusercontent.com/102650331/169689104-b80860fc-280a-47a0-a82e-895d632f11fc.png)


# Share your app
- It works in 3 simple steps:
    - Put your app in a public GitHub repo (and make sure it has a requirements.txt!)
    - Sign into share.streamlit.io
    - Click 'Deploy an app' and then paste in your GitHub URL



