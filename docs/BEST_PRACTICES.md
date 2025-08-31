# Best Practices

## Threading and Performance

### Main Thread Guidelines
- Keep the main thread free for UI operations
- Avoid long-running operations on main thread
- Use proper coroutine dispatchers
```kotlin
// Good
viewModelScope.launch(Dispatchers.IO) {
    // Long running operation
    withContext(Dispatchers.Main) {
        // Update UI
    }
}

// Bad
viewModelScope.launch {
    // Long running operation on Main thread
}
```

### Coroutines Best Practices

#### 1. Structured Concurrency
```kotlin
// Good
viewModelScope.launch {
    val result = async { repository.getData() }
    val processedData = result.await()
}

// Bad
GlobalScope.launch { // Avoid GlobalScope
    repository.getData()
}
```

#### 2. Error Handling
```kotlin
viewModelScope.launch {
    try {
        val result = withContext(Dispatchers.IO) {
            repository.getData()
        }
        processResult(result)
    } catch (e: Exception) {
        handleError(e)
    }
}
```

#### 3. Cancellation
```kotlin
private var searchJob: Job? = null

fun search(query: String) {
    searchJob?.cancel() // Cancel previous job
    searchJob = viewModelScope.launch {
        delay(300) // Debounce
        performSearch(query)
    }
}
```

### Kotlin Flows

#### 1. Flow Collection
```kotlin
// In ViewModel
private val _state = MutableStateFlow(UiState())
val state = _state.asStateFlow()

// In Composable
val state by viewModel.state.collectAsStateWithLifecycle()
```

#### 2. Flow Transformations
```kotlin
val processedData = repository.getData()
    .map { it.process() }
    .filter { it.isValid }
    .flowOn(Dispatchers.Default)
    .catch { emit(ErrorState(it)) }
```

#### 3. Flow Combination
```kotlin
combine(
    userFlow,
    settingsFlow
) { user, settings ->
    UiState(user, settings)
}.stateIn(
    scope = viewModelScope,
    started = SharingStarted.WhileSubscribed(5000),
    initialValue = UiState()
)
```

## Memory Management

### 1. Avoiding Memory Leaks

#### ViewModel Scope
```kotlin
class FeatureViewModel : ViewModel() {
    init {
        viewModelScope.launch {
            // This will be automatically cancelled
        }
    }
}
```

#### Composable Scope
```kotlin
@Composable
fun MyScreen() {
    val scope = rememberCoroutineScope()
    LaunchedEffect(Unit) {
        // This will be cancelled when the composable leaves composition
    }
}
```

### 2. Resource Cleanup
```kotlin
class FeatureViewModel : ViewModel() {
    private var job: Job? = null
    
    fun startOperation() {
        job?.cancel() // Cancel previous job
        job = viewModelScope.launch {
            // New operation
        }
    }
    
    override fun onCleared() {
        job?.cancel()
        super.onCleared()
    }
}
```

### 3. Context References
```kotlin
// Bad
class LeakyViewModel(
    private val context: Context // Don't store Context
) : ViewModel()

// Good
class SafeViewModel(
    private val repository: Repository
) : ViewModel()
```

## State Management

### 1. UI State
```kotlin
data class UiState<T>(
    val isLoading: Boolean = false,
    val data: T? = null,
    val error: String? = null
)
```

### 2. State Updates
```kotlin
private val _state = MutableStateFlow(UiState<Data>())
val state = _state.asStateFlow()

fun updateState(newData: Data) {
    _state.update { it.copy(data = newData) }
}
```

### 3. State Restoration
```kotlin
class FeatureViewModel(
    private val savedStateHandle: SavedStateHandle
) : ViewModel() {
    private val _state = savedStateHandle.getStateFlow("key", UiState())
}
```

## Testing Guidelines

### 1. Unit Tests
- Test each layer independently
- Use fake repositories for ViewModels
- Test error scenarios
- Verify state updates

### 2. UI Tests
- Test key user flows
- Verify UI state updates
- Test error states
- Use test tags for finding elements
