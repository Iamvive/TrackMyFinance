# UI Guidelines

## Screen Structure

Each screen in the app should follow this structure:

### 1. ComposeView File (`FeatureScreen.kt`)
```kotlin
@Composable
fun FeatureScreen(
    modifier: Modifier = Modifier,
    viewModel: FeatureViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
    val effectFlow = viewModel.effect.collectAsStateWithLifecycle()
    
    // Handle UI effects
    LaunchedEffect(Unit) {
        effectFlow.value?.let { effect ->
            // Handle effect
        }
    }
    
    FeatureContent(
        state = state,
        onEvent = viewModel::onEvent,
        modifier = modifier
    )
}

@Composable
private fun FeatureContent(
    state: FeatureState,
    onEvent: (FeatureEvent) -> Unit,
    modifier: Modifier = Modifier
)
```

### 2. ViewModel (`FeatureViewModel.kt`)
```kotlin
@HiltViewModel
class FeatureViewModel @Inject constructor(
    private val featureUseCase: FeatureUseCase,
    private val savedStateHandle: SavedStateHandle
) : ViewModel() {
    private val _state = MutableStateFlow(FeatureState())
    val state = _state.asStateFlow()

    private val _effect = MutableSharedFlow<FeatureEffect?>()
    val effect = _effect.asStateFlow()

    fun onEvent(event: FeatureEvent) {
        when (event) {
            // Handle events
        }
    }
}
```

### 3. State (`FeatureState.kt`)
```kotlin
data class FeatureState(
    val isLoading: Boolean = false,
    val error: String? = null,
    val data: Data? = null
)
```

### 4. Effects (`FeatureEffect.kt`)
```kotlin
sealed interface FeatureEffect {
    data class ShowSnackbar(val message: String) : FeatureEffect
    data class Navigate(val route: String) : FeatureEffect
}
```

### 5. Events (`FeatureEvent.kt`)
```kotlin
sealed interface FeatureEvent {
    data class UserAction(val data: String) : FeatureEvent
    object Refresh : FeatureEvent
}
```

## Best Practices

### Composable Functions
- Keep composables small and focused
- Use preview annotations
- Extract reusable components
- Follow unidirectional data flow

### State Management
- Use `rememberSaveable` for configuration changes
- Handle process death with SavedStateHandle
- Use collectAsStateWithLifecycle for Flow collection

### Navigation
- Use type-safe arguments
- Handle deep links appropriately
- Use proper navigation patterns

### Performance
- Use remember and derived state appropriately
- Avoid unnecessary recomposition
- Use LaunchedEffect for side effects

### Theming
- Use Material3 components
- Follow design system guidelines
- Support dynamic colors
- Support dark theme
