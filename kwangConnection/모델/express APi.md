[[mongoDB 모델]]

localhost 3000/
# get
	/api/nonce
	32바이트길이의 랜덤한 논스값을 받습니다.
	메타마스크 서명기능을 사용하여 사용자가 프라이빗키를 가지고 있는 적합한 사용자인지 검사합니다

	/posts/
	데이터베이스의 postlist에 있는 포스트들을 불러옵니다. 
	홈페이지의 메인 페이지에서 사용됩니다.

	/posts/topFeeds/:number/:sort 
	postList의 포스트들을 줄 세워 :number값만큼의 포스트들을 가져옵니다.
	:sort 에는 view(조회수), vote(추천수)로 정렬할 수 있습니다

	/posts/:Category
	카테고리별로 포스트들의 목록을 가져옵니다./
	현재는 Anything(잡담), private(비밀), Promotio(홍보)카테고리로 나뉩니다

	/posts/:Category/:Index
	특정 카테고리에 저장된 인덱스 번호의 포스트를 호출합니다.
	보통 게시판에서 읽고싶은 포스트를 클릭했을때 요청됩니다.

# post 
	/posts/comments/:Category/:Index
	선택된 카테고리의 인덱스 글의 댓글을 달때 사용되는 api입니다. 

	/posts/:Category 
	선택된 카테고리에 새 글을 쓸때 사용됩니다

	/users/newUser
	유저가 비밀키로 작성된 문자열, 원본 메세지, 공개주소 를 보내면, 해당 유저의 공개키가 
	데이터베이스에 등록되어 있는지 확인합니다.
	없을시에 false를 보냅니다.
	그럴경우, 브라우저에서는 메일인증을 통한 api를 호출하여 메일 인증을 진행하여 새로 등록할 수 있습니다.
	만약 이미 등록되어 있을경우, true를 보내 유저는 웹사이트에 접속할 수 있습니다.

# Delete
	/posts/:Category/:index
	선택된 카테고리의 인덱스의 글을 삭제할때 사용됩니다. 