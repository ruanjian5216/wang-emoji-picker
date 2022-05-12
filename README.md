### install

```
wang-emoji-picker
```

### use

index.html 

```
<script src="https://twemoji.maxcdn.com/v/latest/twemoji.min.js" crossorigin="anonymous"></script>
```

main.js

```
import Vue from 'vue'
import wangEmojiPicker from "wang-emoji-picker";
Vue.use(wangEmojiPicker)
```

```
 <wang-emoji-picker @emojiClickItem="emojiClickItem"  />
```

### methods

`emojiClickItem` 会将选中的图标和图片发出