# Android-Code-Rule
1：所有color、dimens、strings统一抽取到xml配置文件中，注意命名规范,所有ui图片资源统一放到mipmap-xhdpi中；

2：关于注解采用的是ButterKnife，用法详见如下：

     //绑定控件
    @BindView(R.id.btn_facebook)
    Button btn_facebook;

    @BindView(R.id.btn_youke)
    Button btn_youke;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        //ButterKnife绑定
        ButterKnife.bind(this);

        setViews();
    }

    private void setViews() {
        btn_facebook.setOnClickListener(this);
        btn_youke.setOnClickListener(this);
    }
    
3：关于图片加载采用的是Glide，用法详见如下

  Glide.with(context).load("http://inthecheesefactory.com/uploads/source/glidepicasso/cover.jpg").into(ivImg);
  
  加载图片的方法会统一抽取到HttpManager：
  
   	public void loadImage(String img_url, final ImageView imageView) {
		if(!TextUtils.isEmpty(img_url)){
			Glide.with(context).load(img_url).dontAnimate().error(R.drawable.icon).placeholder(R.drawable.img_default_friend).into(imageView);
		}else {
			imageView.setImageResource(R.drawable.icon);
		}
	}
 
4：关于json数据格式解析采用的是Gson，用法详见如下：
  
        String json = "4";
        Gson gson = new Gson();
        gson.fromJson(json,XXXX.class);
  
5：关于联网采用的是okHttp和okIo，用法详见如下：

  //同步请求
	public Result<String> requestAlipay(float price) {
		final String path = "/pay/ali_order_param" + getUserinfo();
		HttpRequest request = new HttpRequest(0, HttpRequest.METHOD_POST, path);
		request.add("money", String.valueOf(price));
		return request(request, new StringResultParser());
	}

  //异步请求
	public void requestSignUp(String anchorUid, String name, String mobile,
			String company, String position, ICallback<Result<String>> callback) {
		final String path = "/activity/set_participate" + getUserinfo();
		HttpRequest request = new HttpRequest(0, HttpRequest.METHOD_POST, path);
		request.add("anchor_uid", anchorUid);
		request.add("name", name);
		request.add("mobile", mobile);
		request.add("company", company);
		request.add("position", position);

		request(request, new StringResultParser(), callback);
	}
  
6：关于列表显示控件采用的是RecyclerView结合RecyclerAdapter，用法详见如下：

        MainRecyclerAdapter recyclerAdapter = new MainRecyclerAdapter(mDatas, this);
        recyclerView.setAdapter(recyclerAdapter);

        recyclerView.setLayoutManager(new LinearLayoutManager(this ));

        recyclerAdapter.setOnItemClickListener(this);

/**
 * Created by yyxk-hs on 2016/11/16.
 */
 
public class MainRecyclerAdapter extends RecyclerView.Adapter<MainRecyclerAdapter.MyViewHolder> {

    private final List<String> mDatas;
    private final Context mContext;

    private OnRecyclerClickListener onRecyclerClickListener;

    public MainRecyclerAdapter(List<String> mDatas, Context mContext) {
        this.mDatas = mDatas;
        this.mContext = mContext;
    }

    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup viewGroup, int i) {
        View view = LayoutInflater.from(mContext).inflate(R.layout.item_list, viewGroup, false);
        MyViewHolder holder = new MyViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(MyViewHolder viewHolder, int i) {
        viewHolder.tv_item.setText(mDatas.get(i));
    }

    @Override
    public int getItemCount() {
        return mDatas.size();
    }


    class MyViewHolder extends RecyclerView.ViewHolder {

        TextView tv_item;

        public MyViewHolder(View itemView) {
            super(itemView);

            setItemViews(itemView);
        }

        private void setItemViews(View itemView) {
            tv_item = (TextView) itemView.findViewById(R.id.tv_item);
            tv_item.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if(onRecyclerClickListener != null){
                        onRecyclerClickListener.onItemClick(0, getPosition());
                    }
                }
            });
        }
    }

    //自定义条目点击监听
    public void setOnItemClickListener(OnRecyclerClickListener listener){
        this.onRecyclerClickListener = listener;
    }
}
/**
 * Created by yyxk-hs on 2016/11/16.
 * type 点击条目类型
 * position 点击条目索引
 */
 
public interface OnRecyclerClickListener {
    void onItemClick(int type, int position);
}



  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
