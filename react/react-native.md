# React Native

## General

- `<ScrollView>` for scrollable views
- all texts in `<Text>`
- `<SafeAreaView>` around Views to deal with irregular top of the screen on different devices
- styles only inline (cascade in array)

  ```
  <Text style={[styles.mainText]}>
    some text
  </Text>

  const styles = StyleSheet.create({
    mainText: {
      fontSize: 20,
      fontWeight: "600"
    }
  })
  ```

- `flex: 1` for same size as parent
- `FlatList` is better than .map()

  ```
  <FlatList
      data={arrayWithData}
      keyExtractor={item => item.timestamp}
      renderItem={({ item }) => <Component element={item} />}
  />
  ```

  - if data already has string id keyExtractor not necessary
  - `listHeaderComponent={}` ...
  - scrollable
  - pull to refresh

    ```
    import { RefreshControl } from 'react-native';

    const [isRefreshing, setIsRefreshing] = useState(false);

    const handleRefresh = useCallback(async ()  => {
      setIsRefreshing(true);
      await handleFetch();
      setIsRefreshing(false);
    });

    <RefreshControl refreshing={isRefreshing} onRefresh={handleRefresh} />
    ```

- TouchableOpacity around pressable component (old)
- `<Pressable>` is modern, but has to be styled (ternary operator for style according to state)
- to get properties of user device `useWindowDimensions()`
- Victory for charts

## Navigation

- @react-navigation/native
- `<NavigationContainer>` wraps
- navigators can be nested (modals `mode="modal`)

### Stack navigation

```
const Stack = createStackNavigator();

<NavigationContainer>
  <Stack.Navigator>
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Screen2" component={Screen2} />
  </Stack.Navigator>
</NavigationContainer>
```

- every component inside has navigation prop
  `navigation.navigate("ScreenName")`
- to `.navigate` second arg can be passed and its value will be available in opened component in route.params

  ```
  navigation.navigate('ColorPalette', { paletteName: 'Solarized" })

  route.params.paletteName
  ```

- for dynamic screen titles
  ```
  <StackScreen name="Profile" component={ProfileScreen} options={({ route }) => ({title: route.params.name})} />
  ```

### Bottom Navigation

```
npm install @react-navigation/native react-native-screens react-native-safe-area-context @react-navigation/bottom-tabs
```

```
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const BottomTabs = createBottomTabNavigator();

export const BottomTabsNavigator: React.FC = () => {
  return (
    <BottomTabs.Navigator>
      <BottomTabs.Screen name="Home" component={Home} />
      <BottomTabs.Screen name="History" component={History} />
    </BottomTabs.Navigator>
  );
};
```

## Forms

- TextInput
  - `keyboardType="numeric"`
  - `secureTextEntry={true}`
  - multilnes
    - `multiline={true}`
    - `numberOfLines=4`
  - controlled form
    - `value={vlaue}`
    - `onChangeText={setValue}`
- `<Picker>` with `<Picker.item>`
- `<Switch>`

## Context

```
const AppContext = createContext<AppContextType>(defaultValue);

export const AppProvider = ({ children }: PropsWithChildren) => {
  const [value, setValue] = useState()
  return (
    <AppContext.Provider value={{ value, setValue }}>
      {children}
    </AppContext.Provider>
  );
};
```

- wrap `<NavigationContainer>` with `<AppProvider>`
- then nice way is to create a hook
  ```
  export const useAppContext = () => React.useContext(AppContext);
  ```

## Persisting Data - Async Storage

- npm install @react-native-async-storage/async-storage

```
type AppData = {
  ...
};

const key = "app_key";

const setAppData = async (appData: AppData) => {
  try {
    await AsyncStorage.setItem(key, JSON.stringify(appData));
  } catch {}
};

const getAppData = async (): Promise<AppData | null> => {
  try {
    const res = await AsyncStorage.getItem(key);
    if (res) {
      return JSON.parse(res);
    }
  } catch {}
  return null;
};
```

## Images

- local images
  - `<Image>` from react-native
  ```
  const imageSrc = require('../../assets/butterflies.png');
  <Image source={imageSrc} />;
  ```
  - width/heihgt values related to pixel density
  - convention 3 sizes per image
  - automatically when in the same folder with image.png
    - image@2x.png
    - image@3x.png
  - sizing - height: 100, aspectRatio: 1
- network images
  - `<Image source={{uri : imageUrl}} />`
  - it needs height/width in style otherwise wont render
- `<ImageBackground />`
- for production ract-native-fast-image
- react-native-svg
  - flaticon.com
  - Svg and Path from react-native
  - tool for converting to sensible svg paths react-svgr.com

## Animation

### Basic Animations for UI Shifts

- LayoutAnimation from react-native
- enabling on Android
  ```
  import { Platform, UIManager } from 'react-native';
  if (Platform.OS === 'android') {
    if (UIManager.setLayoutAnimationEnabledExperimental) {
      UIManager.setLayoutAnimationEnabledExperimental(true);
    }
  }
  ```
- `LayoutAnimation.configureNext(LayoutAnimation.Presets.easeInEaseOut);` before function that causes UI change (like deleting an item)
-

### Reanimated 2

- react-native-reanimated
- in babel-config.js `plugins: ['react-native-reanimated/plugin']`
- only reanimated components can use animated styles
  ```
  const ReanimatedPressable = Reanimated.createAnimatedComponent(Pressable);
  ```
- useAnimatedStyle

  ```
  const buttonStyle = useAnimatedStyle(
    () => ({
      opacity: selectedMood ? withTiming(1) : withTiming(0.5),

    }),
    [selectedMood]
  );
  ```

- `withTiming(value, timeLength)` - makes transitions
