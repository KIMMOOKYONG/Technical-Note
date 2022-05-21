# Main concepts
```
# 3가지 방식의 Streamlit 프로그램을 실행시키는 방식

# (1)
# 작성한 스크립트에 파라미터를 전달하는 경우 반드시 -- 사용해야함.
# -- 사용하지 않으면 Sreamlit의 파라미터로 인식함.
streamlit run your_script.py [-- script args]

# (2)
# Python module 실행하는 방법
python -m streamlit run your_script.py

# (3)
# github 에 존재하는 스크립트를 실행하는 방식도 지원함.
streamlit run https://raw.githubusercontent.com/streamlit/demo-uber-nyc-pickups/master/streamlit_app.py

```

![image](https://user-images.githubusercontent.com/102650331/169661224-c6ca4da4-90d6-4ab7-8edb-b467e1aa0d2c.png)

# Development flow
```
소스코드 변경 후 저장하면 Streamlit 프로그램일 자동으로 감지해서 재실행 여부를 묻거나 
설정에서 Always rerun 옵션을 선택하면 자동으로 변경되 소스 코드를 반영해주는 기능을 제공한다.

```

# Data flow
```
소스 코드 변경시 또는 컴포넌트 이벤트 발생시 소스코드 전체를 재실행
재실행 시 비용이 많은 부분에 대해서는 @st.cache 사용

```

# Display and style data
## Use magic("magic commands")
```python
"""
# My first app
Here's our first attempt at using data to create a table:
"""

import streamlit as st
import pandas as pd

df = pd.DataFrame({
  'first column': [1, 2, 3, 4],
  'second column': [10, 20, 30, 40]
})

# 내부적으로 st.write() 메서드를 이용해서 출력함
df

```
![image](https://user-images.githubusercontent.com/102650331/169661876-1e74ff8a-7d20-4fe0-8c52-9440c449427b.png)


## Write a data frame
```python
import streamlit as st
import pandas as pd

st.write("Here's our first attempt at using data to create a table:")
st.write(pd.DataFrame({
    'first column': [1, 2, 3, 4],
    'second column': [10, 20, 30, 40]
}))

```

```python
import streamlit as st
import numpy as np

dataframe = np.random.randn(10, 20)
st.dataframe(dataframe)

```

```python
import streamlit as st
import numpy as np
import pandas as pd

dataframe = pd.DataFrame(
    np.random.randn(10, 20),
    columns=('col %d' % i for i in range(20)))

st.dataframe(dataframe.style.highlight_max(axis=0))

```

```python
import streamlit as st
import numpy as np
import pandas as pd

dataframe = pd.DataFrame(
    np.random.randn(10, 20),
    columns=('col %d' % i for i in range(20)))
st.table(dataframe)

```