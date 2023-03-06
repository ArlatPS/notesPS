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

## Navigation

- @react-navigation/native
- `<NavigationContainer>` wraps
- navigators can be nested (modals `mode="modal`)
- stack navigation

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
