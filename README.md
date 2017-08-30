# Kotlin Extensions
Curated list of Most commonly used Kotlin Extensions.

```kotlin
/**
 * Extension method to remove the required boilerplate for running code after a view has been
 * inflated and measured.
 *
 * @author Antonio Leiva
 * @see <a href="https://antonioleiva.com/kotlin-ongloballayoutlistener/>Kotlin recipes: OnGlobalLayoutListener</a>
 */
inline fun <T : View> T.afterMeasured(crossinline f: T.() -> Unit) {
    viewTreeObserver.addOnGlobalLayoutListener(object : ViewTreeObserver.OnGlobalLayoutListener {
        override fun onGlobalLayout() {
            if (measuredWidth > 0 && measuredHeight > 0) {
                viewTreeObserver.removeOnGlobalLayoutListener(this)
                f()
            }
        }
    })
}
```



```kotlin
/**
 * Extension method to simplify the code needed to apply spans on a specific sub string.
 */
inline fun SpannableStringBuilder.withSpan(vararg spans: Any, action: SpannableStringBuilder.() -> Unit):
        SpannableStringBuilder {
    val from = length
    action()

    for (span in spans) {
        setSpan(span, from, length, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE)
    }

    return this
}
```



```kotlin
/**
 * Extension method to provide simpler access to {@link ContextCompat#getColor(int)}.
 */
fun Context.getColorCompat(color: Int) = ContextCompat.getColor(this, color)
```



```kotlin
/**
 * Extension method to provide simpler access to {@link View#getResources()#getString(int)}.
 */
fun View.getString(stringResId: Int): String = resources.getString(stringResId)
```



```kotlin
/**
 * Extension method to provide show keyboard for View.
 */
fun View.showKeyboard() {
    val imm = context.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
    this.requestFocus()
    imm.showSoftInput(this, 0)
}
```



```kotlin
/**
 * Extension method to provide hide keyboard for [Activity].
 */
fun Activity.hideSoftKeyboard() {
    if (currentFocus != null) {
        val inputMethodManager = getSystemService(Context
                .INPUT_METHOD_SERVICE) as InputMethodManager
        inputMethodManager.hideSoftInputFromWindow(currentFocus!!.windowToken, 0)
    }
}
```



```kotlin
/**
 * Extension method to provide hide keyboard for [Fragment].
 */
fun Fragment.hideSoftKeyboard() {
    activity?.hideSoftKeyboard()
}
```



```kotlin
/**
 * Extension method to provide hide keyboard for [View].
 */
fun View.hideKeyboard() {
    val imm = context.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
    imm.hideSoftInputFromWindow(windowToken, 0)
}
```



```kotlin
/**
 * Extension method to int time to 2 digit String
 */
fun Int.twoDigitTime() = if (this < 10) "0" + toString() else toString()

```



```kotlin
/**
 * Extension method to provide quicker access to the [LayoutInflater] from [Context].
 */
fun Context.getLayoutInflater() = getSystemService(Context.LAYOUT_INFLATER_SERVICE) as LayoutInflater
```



```kotlin
/**
 * Extension method to provide quicker access to the [LayoutInflater] from a [View].
 */
fun View.getLayoutInflater() = context.getLayoutInflater()
```



```kotlin
/**
 * Extension method to replace all text inside an [Editable] with the specified [newValue].
 */
fun Editable.replaceAll(newValue: String) {
    replace(0, length, newValue)
}
```



```kotlin
/**
 * Extension method to replace all text inside an [Editable] with the specified [newValue] while
 * ignoring any [android.text.InputFilter] set on the [Editable].
 */
fun Editable.replaceAllIgnoreFilters(newValue: String) {
    val currentFilters = filters
    filters = emptyArray()
    replaceAll(newValue)
    filters = currentFilters
}
```



```kotlin
/**
 * Extension method to cast a char with a decimal value to an [Int].
 */
fun Char.decimalValue(): Int {
    if (!isDigit())
        throw IllegalArgumentException("Out of range")
    return this.toInt() - '0'.toInt()
}
```



```kotlin
/**
 * Extension method to simplify view binding.
 */
fun <T : ViewDataBinding> View.bind() = DataBindingUtil.bind<T>(this) as T
```



```kotlin
/**
 * Extension method to simplify view inflating and binding inside a [ViewGroup].
 *
 * e.g.
 * This:
 *<code>
 *     binding = bind(R.layout.widget_card)
 *</code>
 *
 * Will replace this:
 *<code>
 *     binding = DataBindingUtil.inflate(getLayoutInflater(), R.layout.widget_card, this, true)
 *</code>
 */
fun <T : ViewDataBinding> ViewGroup.bind(layoutId: Int): T {
    return DataBindingUtil.inflate(getLayoutInflater(), layoutId, this, true)
}
```



```kotlin
/**
 * Extension method to get Date for String with specified format.
 */
fun String.dateInFormat(format: String): Date? {
    val dateFormat = SimpleDateFormat(format, Locale.US)
    var parsedDate: Date? = null
    try {
        parsedDate = dateFormat.parse(this)
    } catch (ignored: ParseException) {
        ignored.printStackTrace()
    }
    return parsedDate
}
```



```kotlin
/**
 * Extension method to get ClickableSpan.
 * e.g.
 * val loginLink = getClickableSpan(context.getColorCompat(R.color.colorAccent), { })
 */
fun getClickableSpan(color: Int, action: (view: View) -> Unit): ClickableSpan {
    return object : ClickableSpan() {
        override fun onClick(view: View) {
            action
        }

        override fun updateDrawState(ds: TextPaint) {
            super.updateDrawState(ds)
            ds.color = color
        }
    }
}
```



```kotlin
/**
 * Extension method to get ClickableSpan.
 * e.g.
 * val loginLink = getClickableSpan(context.getColorCompat(R.color.colorAccent), { })
 */
fun View.doOnLayout(onLayout: (View) -> Boolean) {
    addOnLayoutChangeListener(object : View.OnLayoutChangeListener {
        override fun onLayoutChange(view: View, left: Int, top: Int, right: Int, bottom: Int,
                oldLeft: Int, oldTop: Int, oldRight: Int, oldBottom: Int) {
            if (onLayout(view)) {
                view.removeOnLayoutChangeListener(this)
            }
        }
    })
}
```



```kotlin
/**
 * Extension method to get ClickableSpan.
 * e.g.
 * val loginLink = getClickableSpan(context.getColorCompat(R.color.colorAccent), { })
 */
fun View.updatePadding(paddingStart: Int = getPaddingStart(),
        paddingTop: Int = getPaddingTop(),
        paddingEnd: Int = getPaddingEnd(),
        paddingBottom: Int = getPaddingBottom()) {
    setPaddingRelative(paddingStart, paddingTop, paddingEnd, paddingBottom)
}
```



```kotlin
/**
 * Extension method to get ClickableSpan.
 * e.g.
 * val loginLink = getClickableSpan(context.getColorCompat(R.color.colorAccent), { })
 */
fun ImageView.loadFromUrl(imageUrl: String) {
    Glide.with(this).load(imageUrl).into(this)
}
```



```kotlin
/**
 * Extension method to get ClickableSpan.
 * e.g.
 * val loginLink = getClickableSpan(context.getColorCompat(R.color.colorAccent), { })
 */
fun MenuItem.loadIconFromUrl(context: Context, imageUrl: String) {
    Glide.with(context).asBitmap()
            .load(imageUrl)
            .into(object : SimpleTarget<Bitmap>(100, 100) {
                override fun onResourceReady(resource: Bitmap?, transition: Transition<in Bitmap>?) {
                    val circularIcon = RoundedBitmapDrawableFactory.create(context.resources, resource)
                    circularIcon.isCircular = true
                    icon = circularIcon
                }
            })
}
```



```kotlin
/**
 * Extension method to get ClickableSpan.
 * e.g.
 * val loginLink = getClickableSpan(context.getColorCompat(R.color.colorAccent), { })
 */
inline fun SharedPreferences.edit(preferApply: Boolean = false, f: SharedPreferences.Editor.() -> Unit) {
    val editor = edit()
    editor.f()
    if (preferApply) editor.apply() else editor.commit()
}
```



```kotlin
/**
 * The `fragment` is added to the container view with id `frameId`. The operation is
 * performed by the `fragmentManager`.
 */
fun AppCompatActivity.replaceFragmentInActivity(fragment: Fragment, @IdRes frameId: Int) {
    supportFragmentManager.transact {
        replace(frameId, fragment)
    }
}
```



```kotlin
/**
 * The `fragment` is added to the container view with tag. The operation is
 * performed by the `fragmentManager`.
 */
fun AppCompatActivity.addFragmentToActivity(fragment: Fragment, tag: String) {
    supportFragmentManager.transact {
        add(fragment, tag)
    }
}
```



```kotlin
/**
 * Setup actionbar
 */
fun AppCompatActivity.setupActionBar(@IdRes toolbarId: Int, action: ActionBar.() -> Unit) {
    setSupportActionBar(findViewById(toolbarId))
    supportActionBar?.run {
        action()
    }
}
```



```kotlin
/**
 * Runs a FragmentTransaction, then calls commit().
 */
private inline fun FragmentManager.transact(action: FragmentTransaction.() -> Unit) {
    beginTransaction().apply {
        action()
    }.commit()
}
```



```kotlin
/**
 * Transforms static java function Snackbar.make() to an extension function on View.
 */
fun View.showSnackbar(snackbarText: String, timeLength: Int) {
    Snackbar.make(this, snackbarText, timeLength).show()
}
```



```kotlin
/**
 * Get view model for Activity
 */
fun <T : ViewModel> AppCompatActivity.obtainViewModel(viewModelClass: Class<T>) =
        ViewModelProviders.of(this, ViewModelFactory.getInstance(application)).get(viewModelClass)
```



```kotlin
/**
 * Triggers a snackbar message when the value contained by snackbarTaskMessageLiveEvent is modified.
 */
fun View.setupSnackbar(lifecycleOwner: LifecycleOwner,
        snackbarMessageLiveEvent: SingleLiveEvent<Int>, timeLength: Int) {
    snackbarMessageLiveEvent.observe(lifecycleOwner, Observer {
        it?.let { showSnackbar(context.getString(it), timeLength) }
    })
}
```

```kotlin
fun View.setPaddingLeft(value: Int) = setPadding(value, paddingTop, paddingRight, paddingBottom)
```


```kotlin
fun View.setPaddingRight(value: Int) = setPadding(paddingLeft, paddingTop, value, paddingBottom)
```


```kotlin
fun View.setPaddingTop(value: Int) = setPaddingRelative(paddingStart, value, paddingEnd, paddingBottom)
```


```kotlin
fun View.setPaddingBottom(value: Int) = setPaddingRelative(paddingStart, paddingTop, paddingEnd, value)
```


```kotlin
fun View.setPaddingStart(value: Int) = setPaddingRelative(value, paddingTop, paddingEnd, paddingBottom)
```


```kotlin
fun View.setPaddingEnd(value: Int) = setPaddingRelative(paddingStart, paddingTop, value, paddingBottom)
```


```kotlin
fun View.setPaddingHorizontal(value: Int) = setPaddingRelative(value, paddingTop, value, paddingBottom)
```


```kotlin
fun View.setPaddingVertical(value: Int) = setPaddingRelative(paddingStart, value, paddingEnd, value)
```


```kotlin
fun View.setHeight(value: Int) {
    val lp = layoutParams
    lp?.let {
        lp.height = value
        layoutParams = lp
    }
}
```


```kotlin
fun View.setWidth(value: Int) {
    val lp = layoutParams
    lp?.let {
        lp.width = value
        layoutParams = lp
    }
}
```

```kotlin
fun View.resize(width: Int, height: Int) {
    val lp = layoutParams
    lp?.let {
        lp.width = width
        lp.height = height
        layoutParams = lp
    }
}
```


```kotlin
val ViewGroup.children: List<View>
    get() = (0 until childCount).map { getChildAt(it) }

fun View.animateWidth(toValue: Int, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR): AnimatePropsWrapper {
    if (toValue == width || layoutParams == null) {
        return AnimatePropsWrapper(null)
    }
    return AnimatePropsWrapper(ValueAnimator().apply {
        setIntValues(width, toValue)
        setDuration(duration)
        setInterpolator(interpolator)
        addUpdateListener {
            val lp = layoutParams
            lp.width = it.animatedValue as Int
            layoutParams = lp
        }
        start()
    })
}
```


```kotlin
fun View.animateWidthBy(byValue: Int, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR)
        = animateWidth(width + byValue, duration, interpolator)
```


```kotlin
fun View.animateHeight(toValue: Int, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR): AnimatePropsWrapper {
    if (toValue == height || layoutParams == null) {
        return AnimatePropsWrapper(null)
    }
    return AnimatePropsWrapper(ValueAnimator().apply {
        setIntValues(height, toValue)
        setDuration(duration)
        setInterpolator(interpolator)
        addUpdateListener {
            val lp = layoutParams
            lp.height = it.animatedValue as Int
            layoutParams = lp
        }
        start()
    })
}
```


```kotlin
fun View.animateHeightBy(byValue: Int, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR)
        = animateHeight(height + byValue, duration, interpolator)
```


```kotlin
fun TextView.underLine() {
    paint.flags = paint.flags or Paint.UNDERLINE_TEXT_FLAG
    paint.isAntiAlias = true
}
```


```kotlin
fun TextView.deleteLine() {
    paint.flags = paint.flags or Paint.STRIKE_THRU_TEXT_FLAG
    paint.isAntiAlias = true
}
```


```kotlin
fun TextView.bold() {
    paint.isFakeBoldText = true
    paint.isAntiAlias = true
}
```


```kotlin
fun View.click(block: (View) -> Unit) = setOnClickListener { block(it) }
```


```kotlin
fun View.longClick(block: (View) -> Boolean) = setOnLongClickListener { block(it) }
```


```kotlin
fun View.visiable() {
    if (visibility != View.VISIBLE) {
        visibility = View.VISIBLE
    }
}
```


```kotlin
inline fun View.visiableIf(block: () -> Boolean) {
    if (visibility != View.VISIBLE && block()) {
        visibility = View.VISIBLE
    }
}
```


```kotlin
fun View.invisiable() {
    if (visibility != View.INVISIBLE) {
        visibility = View.INVISIBLE
    }
}
```


```kotlin
inline fun View.invisiableIf(block: () -> Boolean) {
    if (visibility != View.INVISIBLE && block()) {
        visibility = View.INVISIBLE
    }
}
```

```kotlin
fun View.gone() {
    if (visibility != View.GONE) {
        visibility = View.GONE
    }
}
```


```kotlin
inline fun View.goneIf(block: () -> Boolean) {
    if (visibility != View.GONE && block()) {
        visibility = View.GONE
    }
}
```


```kotlin
fun View.animateX(toValue: Float, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR): AnimatePropsWrapper {
    if (toValue == translationX) {
        return AnimatePropsWrapper(null)
    }
    return AnimatePropsWrapper(ValueAnimator().apply {
        setFloatValues(translationX, toValue)
        setDuration(duration)
        setInterpolator(interpolator)
        addUpdateListener { this@animateX.translationX = it.animatedValue as Float }
        start()
    })
}
```


```kotlin
fun View.animateXBy(toValue: Float, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR)
        = animateX(translationX + toValue, duration, interpolator)
```


```kotlin
fun View.animateY(toValue: Float, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR): AnimatePropsWrapper {
    if (toValue == translationY) {
        return AnimatePropsWrapper(null)
    }
    return AnimatePropsWrapper(ValueAnimator().apply {
        setFloatValues(translationY, toValue)
        setDuration(duration)
        setInterpolator(interpolator)
        addUpdateListener { this@animateY.translationY = it.animatedValue as Float }
        start()
    })
}
```


```kotlin
fun View.animateYBy(toValue: Float, duration: Long = DURATION, interpolator: Interpolator = INTERPOLATOR)
        = animateY(translationY + toValue, duration, interpolator)
```

```kotlin
val EditText.value
    get() = text.toString()
```


```kotlin
fun View.getBitmap(): Bitmap {
    val bmp = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888)
    val canvas = Canvas(bmp)
    draw(canvas)
    canvas.save()
    return bmp
}
```


```kotlin
inline fun aboveApi(api: Int, included: Boolean = false, block: () -> Unit) {
    if (Build.VERSION.SDK_INT > if (included) api - 1 else api) {
        block()
    }
}
```


```kotlin
inline fun belowApi(api: Int, included: Boolean = false, block: () -> Unit) {
    if (Build.VERSION.SDK_INT < if (included) api + 1 else api) {
        block()
    }
}
```


```kotlin
fun Bitmap.toBase64(): String {
    var result = ""
    val baos = ByteArrayOutputStream()
    try {
        compress(Bitmap.CompressFormat.JPEG, 100, baos)
        baos.flush()
        baos.close()
        val bitmapBytes = baos.toByteArray()
        result = Base64.encodeToString(bitmapBytes, Base64.DEFAULT)
    } catch (e: IOException) {
        e.printStackTrace()
    } finally {
        try {
            baos.flush()
            baos.close()
        } catch (e: IOException) {
            e.printStackTrace()
        }
    }
    return result
}
```


```kotlin
fun Bitmap.resize(w: Number, h: Number): Bitmap {
    val width = width
    val height = height
    val scaleWidth = w.toFloat() / width
    val scaleHeight = h.toFloat() / height
    val matrix = Matrix()
    matrix.postScale(scaleWidth, scaleHeight)
    if (width > 0 && height > 0) {
        return Bitmap.createBitmap(this, 0, 0, width, height, matrix, true)
    }
    return this
}
```


```kotlin
fun Bitmap.saveFile(path: String) {
    val f = File(path)
    if (!f.exists()) {
        f.createNewFile()
    }
    val stream = FileOutputStream(f)
    compress(Bitmap.CompressFormat.PNG, 100, stream)
    stream.flush()
    stream.close()
}
```


```kotlin
fun findColor(@ColorRes resId: Int) = ContextCompat.getColor(Ext.ctx, resId)
```


```kotlin
fun findDrawable(@DrawableRes resId: Int): Drawable? = ContextCompat.getDrawable(Ext.ctx, resId)
```


```kotlin
fun findColorStateList(@ColorRes resId: Int): ColorStateList? = ContextCompat.getColorStateList(Ext.ctx, resId)
```


```kotlin
fun inflate(@LayoutRes layoutId: Int, parent: ViewGroup?, attachToRoot: Boolean = false) = LayoutInflater.from(Ext.ctx).inflate(layoutId, parent, attachToRoot)!!
```


```kotlin
fun inflate(@LayoutRes layoutId: Int) = inflate(layoutId, null)
```


```kotlin
fun Context.dial(tel: String?) = startActivity(Intent(Intent.ACTION_DIAL, Uri.parse("tel:" + tel)))
```


```kotlin
fun Context.sms(phone: String?, body: String = "") {
    val smsToUri = Uri.parse("smsto:" + phone)
    val intent = Intent(Intent.ACTION_SENDTO, smsToUri)
    intent.putExtra("sms_body", body)
    startActivity(intent)
}
```


```kotlin
fun isMainThread(): Boolean = Looper.myLooper() == Looper.getMainLooper()
```

```kotlin
private enum class DateExpr {
    YEAR, MONTH, DAY,
    HOUR, MINUTE, SECOND,
    WEEK, DAY_YEAR, WEEK_YEAR,
    CONSTELLATION
}

fun Long.date(pattern: String = "yyyy-MM-dd HH:mm:ss"): String? = SimpleDateFormat(pattern, Locale.CHINA).format(this)

fun Long.year() = getData(this, DateExpr.YEAR)

fun Long.month() = getData(this, DateExpr.MONTH)

fun Long.day() = getData(this, DateExpr.DAY)

fun Long.week() = getData(this, DateExpr.WEEK)

fun Long.hour() = getData(this, DateExpr.HOUR)

fun Long.minute() = getData(this, DateExpr.MINUTE)

fun Long.second() = getData(this, DateExpr.SECOND)

fun Long.dayOfYear() = getData(this, DateExpr.DAY_YEAR)

fun Long.weekOfYear() = getData(this, DateExpr.WEEK_YEAR)

fun Long.constellation() = getData(this, DateExpr.CONSTELLATION)

fun Int.isLeapYear() = (this % 4 == 0) && (this % 100 != 0) || (this % 400 == 0)
```


```kotlin
inline val connectivityManager: ConnectivityManager
    get() = Ext.ctx.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
```


```kotlin
inline val alarmManager: AlarmManager
    get() = Ext.ctx.getSystemService(Context.ALARM_SERVICE) as AlarmManager
```


```kotlin
inline val telephonyManager: TelephonyManager
    get() = Ext.ctx.getSystemService(Context.TELEPHONY_SERVICE) as TelephonyManager
```


```kotlin
inline val activityManager: ActivityManager
    get() = Ext.ctx.getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager
```



```kotlin
inline val notificationManager: NotificationManager
    get() = Ext.ctx.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
```


```kotlin
inline val appWidgetManager
    @RequiresApi(Build.VERSION_CODES.LOLLIPOP)
    get() = Ext.ctx.getSystemService(Context.APPWIDGET_SERVICE) as AppWidgetManager
```


```kotlin
inline val inputMethodManager: InputMethodManager
    get() = Ext.ctx.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
```


```kotlin
inline val clipboardManager
    get() = Ext.ctx.getSystemService(Context.CLIPBOARD_SERVICE) as ClipboardManager
```


```kotlin
inline val bluetoothManager: BluetoothManager
    @RequiresApi(Build.VERSION_CODES.JELLY_BEAN_MR2)
    get() = Ext.ctx.getSystemService(Context.BLUETOOTH_SERVICE) as BluetoothManager
```


```kotlin
inline val audioManager
    get() = Ext.ctx.getSystemService(Context.AUDIO_SERVICE) as AudioManager
```


```kotlin
inline val batteryManager
    @RequiresApi(Build.VERSION_CODES.LOLLIPOP)
    get() = Ext.ctx.getSystemService(Context.BATTERY_SERVICE) as BatteryManager
```


```kotlin
fun spannableSize(text: String, textSize: Int, isDip: Boolean, start: Int, end: Int): SpannableString {
    val sp = SpannableString(text)
    sp.setSpan(AbsoluteSizeSpan(textSize, isDip), start, end, Spanned.SPAN_INCLUSIVE_EXCLUSIVE)
    return sp
}
```


```kotlin
fun spannableBold(text: String, start: Int, end: Int): SpannableString {
    val sp = SpannableString(text)
    sp.setSpan(StyleSpan(Typeface.BOLD), start, end, Spanned.SPAN_INCLUSIVE_EXCLUSIVE)
    return sp
}
```


```kotlin
fun spannableColor(text: String, @ColorRes colorId: Int, start: Int, end: Int): SpannableString {
    val sp = SpannableString(text)
    sp.setSpan(ForegroundColorSpan(findColor(colorId)), start, end, Spanned.SPAN_INCLUSIVE_EXCLUSIVE)
    return sp
}
```

```kotlin
fun String.toast(isShortToast: Boolean = true) = toast(this, isShortToast)
```


```kotlin
fun String.md5() = encrypt(this, "MD5")
```


```kotlin
fun String.sha1() = encrypt(this, "SHA-1")
```


```kotlin
fun String.isPhone(): Boolean {
    val p = "^1([34578])\\d{9}\$".toRegex()
    return matches(p)
}
```


```kotlin
fun String.isEmail(): Boolean {
    val p = "^(\\w)+(\\.\\w+)*@(\\w)+((\\.\\w+)+)\$".toRegex()
    return matches(p)
}
```


```kotlin
fun String.isNumeric(): Boolean {
    val p = "^[0-9]+$".toRegex()
    return matches(p)
}
```


```kotlin
fun String.equalsIgnoreCase(other: String) = this.toLowerCase().contentEquals(other.toLowerCase())
```


```kotlin
private fun encrypt(string: String?, type: String): String {
    if (string.isNullOrEmpty()) {
        return ""
    }
    val md5: MessageDigest
    return try {
        md5 = MessageDigest.getInstance(type)
        val bytes = md5.digest(string!!.toByteArray())
        bytes2Hex(bytes)
    } catch (e: NoSuchAlgorithmException) {
        ""
    }
}
```


```kotlin
private fun bytes2Hex(bts: ByteArray): String {
    var des = ""
    var tmp: String
    for (i in bts.indices) {
        tmp = Integer.toHexString(bts[i].toInt() and 0xFF)
        if (tmp.length == 1) {
            des += "0"
        }
        des += tmp
    }
    return des
}
```


```kotlin
inline val Context.displayWidth: Int
    get() = resources.displayMetrics.widthPixels
```


```kotlin
inline val Context.displayHeight: Int
    get() = resources.displayMetrics.heightPixels
```


```kotlin
inline val Context.displayMetricks: DisplayMetrics
    get() = resources.displayMetrics
```


```kotlin
inline val Context.inflater: LayoutInflater
    get() = LayoutInflater.from(this)
```


```kotlin
inline fun <reified T : Any> Context.intent() = Intent(this, T::class.java)
```


```kotlin
inline fun <reified T : Any> Context.intent(body: Intent.() -> Unit): Intent {
    val intent = Intent(this, T::class.java)
    intent.body()
    return intent
}
```


```kotlin
inline fun <reified T : Activity> Context?.startActivity() = this?.startActivity(Intent(this, T::class.java))
```


```kotlin
inline fun <reified T : Service> Context?.startService() = this?.startService(Intent(this, T::class.java))
```


```kotlin
inline fun <reified T : Activity> Context.startActivityWithAnimation(enterResId: Int = 0, exitResId: Int = 0) {
    val intent = Intent(this, T::class.java)
    val bundle = ActivityOptionsCompat.makeCustomAnimation(this, enterResId, exitResId).toBundle()
    ContextCompat.startActivity(this, intent, bundle)
}
```


```kotlin
inline fun <reified T : Activity> Context.startActivityWithAnimation(enterResId: Int = 0, exitResId: Int = 0, intentBody: Intent.() -> Unit) {
    val intent = Intent(this, T::class.java)
    intent.intentBody()
    val bundle = ActivityOptionsCompat.makeCustomAnimation(this, enterResId, exitResId).toBundle()
    ContextCompat.startActivity(this, intent, bundle)
}
```


```kotlin
fun Context?.toast(text: CharSequence, duration: Int = Toast.LENGTH_LONG) = this?.let { Toast.makeText(it, text, duration).show() }
```


```kotlin
fun Context?.toast(@StringRes textId: Int, duration: Int = Toast.LENGTH_LONG) = this?.let { Toast.makeText(it, textId, duration).show() }
```


```kotlin
fun Context.getInteger(@IntegerRes id: Int) = resources.getInteger(id)
```


```kotlin
fun Context.getBoolean(@BoolRes id: Int) = resources.getBoolean(id)
```


```kotlin
fun Context.getColor(@ColorRes id: Int) = ContextCompat.getColor(this, id)
```


```kotlin
fun Context.getDrawable(@DrawableRes id: Int) = ContextCompat.getDrawable(this, id)
```


```kotlin
fun Context.inflateLayout(@LayoutRes layoutId: Int, parent: ViewGroup? = null, attachToRoot: Boolean = false): View
        = LayoutInflater.from(this).inflate(layoutId, parent, attachToRoot)
```


```kotlin
inline val Context.inputManager: InputMethodManager?
    get() = getSystemService(INPUT_METHOD_SERVICE) as? InputMethodManager
```


```kotlin
inline val Context.notificationManager: NotificationManager?
    get() = getSystemService(NOTIFICATION_SERVICE) as? NotificationManager
```


```kotlin
inline val Context.keyguardManager: KeyguardManager?
    get() = getSystemService(KEYGUARD_SERVICE) as? KeyguardManager
```


```kotlin
inline val Context.telephonyManager: TelephonyManager?
    get() = getSystemService(TELEPHONY_SERVICE) as? TelephonyManager
```


```kotlin
inline val Context.devicePolicyManager: DevicePolicyManager?
    get() = getSystemService(DEVICE_POLICY_SERVICE) as? DevicePolicyManager
```


```kotlin
inline val Context.connectivityManager: ConnectivityManager?
    get() = getSystemService(CONNECTIVITY_SERVICE) as? ConnectivityManager
```


```kotlin
inline val Context.alarmManager: AlarmManager?
    get() = getSystemService(ALARM_SERVICE) as? AlarmManager
```


```kotlin
inline val Context.clipboardManager: ClipboardManager?
    get() = getSystemService(CLIPBOARD_SERVICE) as? ClipboardManager
```


```kotlin
inline val Context.jobScheduler: JobScheduler?
    get() = getSystemService(JOB_SCHEDULER_SERVICE) as? JobScheduler
```


```kotlin
inline fun Context.notification(body: NotificationCompat.Builder.() -> Unit): Notification {
    val builder = NotificationCompat.Builder(this)
    builder.body()
    return builder.build()
}
```


```kotlin
fun Context.browse(url: String, newTask: Boolean = false): Boolean {
    try {
        val intent = intent(ACTION_VIEW) {
            data = Uri.parse(url)
            if (newTask) addFlags(FLAG_ACTIVITY_NEW_TASK)
        }
        startActivity(intent)
        return true
    } catch (e: Exception) {
        return false
    }
}
```


```kotlin
fun Context.share(text: String, subject: String = ""): Boolean {
    val intent = Intent()
    intent.type = "text/plain"
    intent.putExtra(EXTRA_SUBJECT, subject)
    intent.putExtra(EXTRA_TEXT, text)
    try {
        startActivity(createChooser(intent, null))
        return true
    } catch (e: ActivityNotFoundException) {
        return false
    }
}
```


```kotlin
fun Context.email(email: String, subject: String = "", text: String = ""): Boolean {
    val intent = intent(ACTION_SENDTO) {
        data = Uri.parse("mailto:")
        putExtra(EXTRA_EMAIL, arrayOf(email))
        if (subject.isNotBlank()) putExtra(EXTRA_SUBJECT, subject)
        if (text.isNotBlank()) putExtra(EXTRA_TEXT, text)
    }
    if (intent.resolveActivity(packageManager) != null) {
        startActivity(intent)
        return true
    }
    return false
}
```


```kotlin
fun Context.makeCall(number: String): Boolean {
    try {
        val intent = Intent(ACTION_CALL, Uri.parse("tel:$number"))
        startActivity(intent)
        return true
    } catch (e: Exception) {
        return false
    }
}
```


```kotlin
fun Context.sendSms(number: String, text: String = ""): Boolean {
    try {
        val intent = intent(ACTION_VIEW, Uri.parse("sms:$number")) {
            putExtra("sms_body", text)
        }
        startActivity(intent)
        return true
    } catch (e: Exception) {
        return false
    }
}
```


```kotlin
fun Context.rate(): Boolean = browse("market://details?id=$packageName") or browse("http://play.google.com/store/apps/details?id=$packageName")
```


```kotlin
fun Fragment?.toast(text: CharSequence, duration: Int = Toast.LENGTH_LONG) = this?.let { activity.toast(text, duration) }
```


```kotlin
fun Fragment?.toast(@StringRes textId: Int, duration: Int = Toast.LENGTH_LONG) = this?.let { activity.toast(textId, duration) }
```


```kotlin
fun SupportFragment?.toast(text: CharSequence, duration: Int = Toast.LENGTH_LONG) = this?.let { activity.toast(text, duration) }
```


```kotlin
fun SupportFragment?.toast(@StringRes textId: Int, duration: Int = Toast.LENGTH_LONG) = this?.let { activity.toast(textId, duration) }
```


```kotlin
inline fun Fragment.notification(body: NotificationCompat.Builder.() -> Unit) = activity.notification(body)
```


```kotlin
inline fun SupportFragment.notification(body: NotificationCompat.Builder.() -> Unit) = activity.notification(body)
```


```kotlin
fun Fragment.browse(url: String, newTask: Boolean = false) = activity.browse(url, newTask)
```


```kotlin
fun SupportFragment.browse(url: String, newTask: Boolean = false) = activity.browse(url, newTask)
```


```kotlin
fun Fragment.share(text: String, subject: String = "") = activity.share(text, subject)
```


```kotlin
fun SupportFragment.share(text: String, subject: String = "") = activity.share(text, subject)
```


```kotlin
fun Fragment.email(email: String, subject: String = "", text: String = "") = activity.email(email, subject, text)
```


```kotlin
fun SupportFragment.email(email: String, subject: String = "", text: String = "") = activity.email(email, subject, text)
```


```kotlin
fun Fragment.makeCall(number: String) = activity.makeCall(number)
```


```kotlin
fun SupportFragment.makeCall(number: String) = activity.makeCall(number)
```


```kotlin
fun Fragment.sendSms(number: String, text: String = "") = activity.sendSms(number, text)
```


```kotlin
fun SupportFragment.sendSms(number: String, text: String = "") = activity.sendSms(number, text)
```


```kotlin
fun Fragment.rate() = activity.rate()
```


```kotlin
fun SupportFragment.rate() = activity.rate()
```
