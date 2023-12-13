
const postList = new Schema({
	postTitle: String,
	userWalletAddress: String,
	timeStamp: Date,
	category: String,
	index: Number,
});

const postDataSchema = new Schema({
	userWalletAddress: String,
	userName: String,
	postTitle: String,
	postContent: String,
	timeStamp: Date,
	view: Number,
	comments: Array,
	vote: Number,
	category: String,
	index: Number,
});

const userDataSchema = new Schema({
	userWalletAddress: String,
	userName: String,
	userPosts: {
	privacy: [],
	Anything: [],
	promotion: [],
},
	nftLists: [],
});