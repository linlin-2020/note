# Activity��PhoneWindow��DecorView��ViewRootImpl �Ĺ�ϵ

1. PhoneWindow��Window ��Ψһ���࣬ÿ��Activity���ᴴ��һ��PhoneWindow��������������Ϊһ�����ڣ������������Ŀ��Ӵ��ڣ�����һ�������࣬��Activity������Viewϵͳ�����Ľӿڣ���Activity��View����ϵͳ���м�㡣

1. DecorView��PhoneWindow��һ���ڲ��࣬������View�㼶����㣬һ������������������������֣�����ݲ�ͬ���������Ե�����ͬ�Ĳ��֡�������setContentView�����б��������������˵����PhoneWindow��installDecor�����б�������

1. ViewRootImpl��DecorView��parent����������View�ĸ����¼�����handleResumeActivity�����б�����

## window�ĸ���

�ٶ�û��window��ֻ��view��ΪUI��ʾ����һ���հ׽����У�ֻ��һ��button��Ȼ�����һ��text view������button��һ���֣���ʾЧ����ʲô����button�����滹��text view�����档��ʾ�㼶����window���ڵ�����֮һ���ֱ��磬ĳ���ڼ䵯��dialog��ʾ�û��������ֵ���popupwindow��ʾ��dialog���棬�ֵ���toast��dialog֮�ϡ���Щ����Ҫĳ��������������Щ��ʾ�Ĳ㼶��

**window���ƾ���Ϊ�˽����Ļ�ϵ�view����ʾ�߼����⡣**

ʲô��window����Android�����У�ÿ��view���ʹ���һ��window��
Ϊʲô����ÿ��view����Ϊÿ��view��view���е���ʾ�����ǹ̶��ģ�����Ϊ���ɷָ�ġ���ɶ�����ڿɷָ�ģ�������һ��activity����ʾһ��dialog��dialog��view����dialog�е�����ӵģ�������activity��view����

> ʲô��view�����������ڲ����и�Activity������һ������xml����ô���Ĳ�����LinearLayout����view���ĸ���������������view�Ͷ��Ǹ�view���Ľڵ㣬�������view���Ͷ�Ӧһ��window��
> 
> �ټ�����������ӣ�
> 
> ���������dialog��ʱ����Ҫ��������view����ô���view���ǲ�����antivity�Ĳ����ڵģ���ͨ��WindowManager��ӵ���Ļ�ϵģ�������activity��view���ڣ��������dialog��һ��������view������������һ��window��
> 
> popupWindow��Ҳ��Ӧһ��window����Ϊ��Ҳ��ͨ��windowManager�����ȥ�ģ�������Activity��view����
> 
> ������ʹ��ʹ��windowManager����Ļ����ӵ��κ�view��������Activity�Ĳ���view������ʹ��ֻ���һ��button��

**view����window���ƵĲ�����λ��ÿһ��view����Ӧһ��window��window��view������(������)��view��window�ı�����ʽ��** ͨ������dialog��popupwindow��activity����window�ı�����ʽ��

window���������ڣ���ֻ��һ�����

�ٸ����ӣ���༯�壬����һ��������Ĵ�����ʽ�����������ѧ������ѧ����������ô����༯��Ҳ�Ͳ����ڡ��������ĺô��ǵõ���һ���µĸ�����ǿ����԰�Ϊ��λ�����Ż��

�ܽ᣺
- window������Ϊ�˽��view��ʾ���ҵ����⣬������view���մ�����ʾ���ڵġ�

- window��window�����еĲ�����λ��ÿ��window��Ӧһ��view��

- window���������ڡ���һ������ĸ��view��window�ı�����ʽ��ÿ��view�ĵ�ǰ״̬������WindowManagerService�е�windowStatus��

***

## window������

### window��type

ǰ�����ǽ���window���ƽ����һ���������view����ʾ�������⣬������Ծ;�����window����ʾ����

window���з���ģ���ͬ������ʾ�߶ȷ�Χ��ͬ�������Ұ�1-1000m�߶ȳ�Ϊ�Ϳգ�1001-2000m�߶ȳ�Ϊ�пգ�2000���ϳ�Ϊ�߿ա�windowҲ��һ�����ո߶ȷ�Χ���з��࣬��Ҳ��һ������Z-Order��������window�ĸ߶ȡ�windowһ���ɷ�Ϊ���ࣺ

- Ӧ�ó��򴰿ڣ�Ӧ�ó��򴰿�һ��λ����ײ㣬Z-Order��1-99

- �Ӵ��ڣ��Ӵ���һ������ʾ��Ӧ�ô���֮�ϣ�Z-Order��1000-1999

- ϵͳ�����ڣ�ϵͳ������һ��λ����㣬���ᱻ������window��ס����Toast��Z-Order��2000-2999�����Ҫ�����Զ���ϵͳ��������Ҫ��̬����Ȩ�ޡ�

Z-OrderԽ��windowԽ�����û���Ҳ����ʾԽ�ߣ��߶ȸߵ�window�Ḳ�Ǹ߶ȵ͵�window��

window��type���Ծ���Z-Order��ֵ�����ǿ��Ը�window��type���Ը�ֵ������window�ĸ߶ȡ�ϵͳΪ��������window��Ԥ���˾�̬���������£����³��ò�������ת�Բο����׵�һƪ���£���

Ӧ�ü�window
```
// Ӧ�ó��� Window �Ŀ�ʼֵ����Сֵ��
public static final int FIRST_APPLICATION_WINDOW = 1;

// Ӧ�ó��� Window �Ļ���ֵ������
public static final int TYPE_BASE_APPLICATION = 1;

// ��ͨ��Ӧ�ó���
public static final int TYPE_APPLICATION = 2;

// �����Ӧ�ó��򴰿ڣ������������ʾ Window ֮ǰʹ����� Window ����ʾһЩ����������
public static final int TYPE_APPLICATION_STARTING = 3;

// TYPE_APPLICATION �ı��壬��Ӧ�ó�����ʾ֮ǰ��WindowManager ��ȴ���� Window �������
public static final int TYPE_DRAWN_APPLICATION = 4;

// Ӧ�ó��� Window �Ľ���ֵ
public static final int LAST_APPLICATION_WINDOW = 99;
```

��window
```
// �� Window ���͵Ŀ�ʼֵ
public static final int FIRST_SUB_WINDOW = 1000;

// Ӧ�ó��� Window ��������塣��Щ Window �������丽�� Window �Ķ�����
public static final int TYPE_APPLICATION_PANEL = FIRST_SUB_WINDOW;

// ������ʾý��(����Ƶ)�� Window����Щ Window �������丽�� Window �ĺ��档
public static final int TYPE_APPLICATION_MEDIA = FIRST_SUB_WINDOW + 1;

// Ӧ�ó��� Window ����������塣��Щ Window �������丽�� Window ���κ�Window�Ķ���
public static final int TYPE_APPLICATION_SUB_PANEL = FIRST_SUB_WINDOW + 2;

// ��ǰWindow�Ĳ��ֺͶ���Window������ͬʱ��������Ϊ�Ӵ�������
public static final int TYPE_APPLICATION_ATTACHED_DIALOG = FIRST_SUB_WINDOW + 3;

// ����ʾý�� Window ���Ƕ����� Window�� ����ϵͳ���ص� API
public static final int TYPE_APPLICATION_MEDIA_OVERLAY  = FIRST_SUB_WINDOW + 4;

// �������Ӧ�ó���Window�Ķ�������ЩWindow��ʾ���丽��Window�Ķ����� ����ϵͳ���ص� API
public static final int TYPE_APPLICATION_ABOVE_SUB_PANEL = FIRST_SUB_WINDOW + 5;

// �� Window ���͵Ľ���ֵ
public static final int LAST_SUB_WINDOW = 1999;
```
ϵͳ��window
```
// ϵͳWindow���͵Ŀ�ʼֵ
public static final int FIRST_SYSTEM_WINDOW = 2000;

// ϵͳ״̬����ֻ����һ��״̬����������������Ļ�Ķ����������������ڶ������ƶ�
public static final int TYPE_STATUS_BAR = FIRST_SYSTEM_WINDOW;

// ϵͳ�������ڣ�ֻ����һ����������������������Ļ�Ķ���
public static final int TYPE_SEARCH_BAR = FIRST_SYSTEM_WINDOW+1;

// �Ѿ���ϵͳ�б��Ƴ�������ʹ�� TYPE_KEYGUARD_DIALOG ����
public static final int TYPE_KEYGUARD = FIRST_SYSTEM_WINDOW+4;

// ϵͳ�Ի��򴰿�
public static final int TYPE_SYSTEM_DIALOG = FIRST_SYSTEM_WINDOW+8;

// ����ʱ��ʾ�ĶԻ���
public static final int TYPE_KEYGUARD_DIALOG = FIRST_SYSTEM_WINDOW+9;

// ���뷨���ڣ�λ����ͨ UI ֮�ϣ�Ӧ�ó�������²������ⱻ�˴��ڸ���
public static final int TYPE_INPUT_METHOD = FIRST_SYSTEM_WINDOW+11;

// ���뷨�Ի�����ʾ�ڵ�ǰ���뷨����֮��
public static final int TYPE_INPUT_METHOD_DIALOG= FIRST_SYSTEM_WINDOW+12;

// ǽֽ
public static final int TYPE_WALLPAPER = FIRST_SYSTEM_WINDOW+13;

// ״̬���Ļ������
public static final int TYPE_STATUS_BAR_PANEL = FIRST_SYSTEM_WINDOW+14;

// Ӧ�ó�����Ӵ�����ʾ�����д���֮��
public static final int TYPE_APPLICATION_OVERLAY = FIRST_SYSTEM_WINDOW + 38;

// ϵͳWindow���͵Ľ���ֵ
public static final int LAST_SYSTEM_WINDOW = 2999;
```

### Window��flags����

flag��־����window�Ķ����Ƚ϶࣬�ܶ����ϵ������ǡ�����window����ʾ�������Ҿ��ò���׼ȷ��

flag���Ƶķ�Χ�����ˣ������龰�µ���ʾ�߼�����������Ϸ�ȣ����д����¼��Ĵ����߼���������ʾȷʵ�����ĺܴ󲿷ֹ��ܣ����ǲ�����ȫ�������濴һ��һЩ���õ�flag����֪��flag�Ĺ����ˣ����³��ò�������ת�Բο����׵�һƪ���£���
```
// �� Window �ɼ�ʱ��������
public static final int FLAG_ALLOW_LOCK_WHILE_SCREEN_ON = 0x00000001;

// Window ��������ݶ��䰵
public static final int FLAG_DIM_BEHIND = 0x00000002;

// Window ���ܻ�����뽹�㣬���������κΰ�����ť�¼�������� Window �� �� EditView����� EditView �� ���ᵯ������̵�
// Window ��Χ����¼�����Ϊԭ���ڴ����������ô������view����Ȼ������Ӧ������ֻҪ�����˴�Flag������������FLAG_NOT_TOUCH_MODAL
public static final int FLAG_NOT_FOCUSABLE = 0x00000008;

// �����˸� Flag,�� Window ֮��İ����¼����͸������ Window ����, ���Լ�ֻ�ᴦ�� Window �����ڵĴ����¼�
// Window ֮��� view Ҳ�ǿ�����Ӧ touch �¼���
public static final int FLAG_NOT_TOUCH_MODAL  = 0x00000020;

// �����˸�Flag����ʾ�� Window ����������κ� touch �¼����������� Window ��������Ӧ��ֻ�ᴫ�������о۽��Ĵ��ڡ�
public static final int FLAG_NOT_TOUCHABLE      = 0x00000010;

// ֻҪ Window �ɼ�ʱ��Ļ�ͻ�һֱ����
public static final int FLAG_KEEP_SCREEN_ON     = 0x00000080;

// ���� Window ռ��������Ļ
public static final int FLAG_LAYOUT_IN_SCREEN   = 0x00000100;

// ���� Window ������Ļ֮��
public static final int FLAG_LAYOUT_NO_LIMITS   = 0x00000200;

// ȫ����ʾ���������е� Window װ�Σ���������Ϸ���������е�ȫ����ʾ
public static final int FLAG_FULLSCREEN      = 0x00000400;

// ��ʾ��FLAG_FULLSCREEN��һ��������ʾ״̬��
public static final int FLAG_FORCE_NOT_FULLSCREEN   = 0x00000800;

// ���û�����������Ļʱ�������绰��������ȥ��Ӧ���¼�
public static final int FLAG_IGNORE_CHEEK_PRESSES    = 0x00008000;

// �򵱰������������� Window ֮��ʱ�������յ�һ��MotionEvent.ACTION_OUTSIDE�¼���
public static final int FLAG_WATCH_OUTSIDE_TOUCH = 0x00040000;

@Deprecated
// ���ڿ����������� Window ֮����ʾ, ʹ�� Activity#setShowWhenLocked(boolean) ��������
public static final int FLAG_SHOW_WHEN_LOCKED = 0x00080000;

// ��ʾ�������ϵͳ��������������ã�ϵͳ������͸���������ƣ�
// �� Window �е���Ӧ������� Window��getStatusBarColor������ Window��getNavigationBarColor������ָ������ɫ��
public static final int FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS = 0x80000000;

// ��ʾҪ��ϵͳ��ֽ��ʾ�ڸ� Window ���棬Window ��������ǰ�͸���ģ�������������������ı�ֽ
public static final int FLAG_SHOW_WALLPAPER = 0x00100000;
```

### window����������

���������������window�Ƚ���ҪҲ�ǱȽϸ��� ������������֮�⻹�м����ճ�����ʹ�õ����ԣ�

- x��y���ԣ�ָ��window��λ��

- alpha��window��͸����

- gravity��window����Ļ�е�λ�ã�ʹ�õ���Gravity��ĳ���

- format��window�����ص��ʽ��ֵ������PixelFormat��

### ��θ�window���Ը�ֵ

window���Եĳ���ֵ�󲿷ִ洢��WindowManager.LayoutParams���У����ǿ���ͨ��������������Щ��������Ȼ����Gravity���PixelFormat��ȡ�

һ����������ǻ�ͨ�����·�ʽ������Ļ�����һ��window��

```
// ��Activity�е���
WindowManager.LayoutParams windowParams = new WindowManager.LayoutParams();
windParams.flags = WindowManager.LayoutParams.FLAG_FULLSCREEN;
TextView view = new TextView(this);
getWindowManager.addview(view,windowParams);
```

���ǿ���ֱ�Ӹ�WindowManager.LayoutParams�����������ԡ�

�ڶ��ָ�ֵ������ֱ�Ӹ�window��ֵ����
```
getWindow().flags = WindowManager.LayoutParams.FLAG_FULLSCREEN;
```

����֮�⣬window��solfInputMode���ԱȽ����⣬������ֱ����AndroidManifest��ָ�������£�
```
<activity android:windowSoftInputMode="adjustNothing" />
```

����ܽ�һ�£�

- window����Ҫ������type��flags��solfInputMode��gravity��

- ���ǿ���ͨ����ͬ�ķ�ʽ��window���Ը�ֵ

- û��Ҫȥȫ����������������������ȥѰ�Ҷ�Ӧ�ĳ�������

## window���ƵĹؼ���