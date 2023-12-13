
[[Redux]]를 사용하기 위해서는
- actions( type값을 필수로 가지는 객체 )
- action Creator(상단의 액션값을 반환하는 함수, 그렇지 않으면 매번 액션값을 작성해야 함)
- reducer (초기값과 액션을 파라미터로 가짐, 여기서 action은 상단에 정의한 actions 객체의 타입을 감지하고, 타입의 종류에 따라 상태를 업데이트 함.
- 여기서 리덕스 toolkit을 사용하여 여러개의 reducer들을 한곳에서 관리하는 store을 만들 수 있음-> configureStore로 호출해서 reducer객체들을 넣어주면 됨
- 

