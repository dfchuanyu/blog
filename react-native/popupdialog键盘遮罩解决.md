### popdialog弹出框组件
[dialog github地址](https://github.com/forrest23/ReactNativeComponents)

### react-native键盘被遮盖的通用办法总结
1. react-native原生组件KeyboardAvoidingView,用KeyboardAvoidingView代替View解决遮盖
~~~
import { KeyboardAvoidingView } from 'react-native';

<KeyboardAvoidingView style={styles.container} behavior="padding" enabled>
  ... 在这里放置需要根据键盘调整位置的组件 ...
</KeyboardAvoidingView>
~~~
2. 第三方组件react-native-keyboard-spacer
yarn add react-native-keyboard-spacer
~~~
import KeyboardSpacer from 'react-native-keyboard-spacer';
...
render() {
    return (
      <View style={[{flex: 1}]}>
        {/* The text input to put on top of the keyboard */}
        <TextInput style={{left: 0, right: 0, height: 45}}
              placeholder={'Enter your text!'}/>
 
        {/* The view that will animate to match the keyboards height */}
        <KeyboardSpacer/>
      </View>
    );
  }
  ~~~
  ### 通过获取键盘高度调整popupdialog的高度
  ~~~
    componentDidMount() {
        this.keyboardDidShowListener = Keyboard.addListener('keyboardDidShow', this._keyboardDidShow.bind(this));
        this.keyboardDidHideListener = Keyboard.addListener('keyboardDidHide', this._keyboardDidHide.bind(this));
    }

    componentWillUnmount() {
        this.keyboardDidShowListener.remove();
        this.keyboardDidHideListener.remove();
    }
    
    _keyboardDidShow(e){
        this.setState({
            keyboardHeight:e.endCoordinates.height
        })
    }

    _keyboardDidHide(e){
        this.setState({
            keyboardHeight:150,
        })
    }
  ~~~
