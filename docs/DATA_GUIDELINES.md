# Data Layer Guidelines

## API Integration

### 1. API Interface
```kotlin
interface FeatureApi {
    @GET("endpoint")
    suspend fun getData(@Query("param") param: String): Response<ApiResponse>
}
```

### 2. Models

#### API Model
```kotlin
@Serializable
data class FeatureApiModel(
    @SerialName("id") val id: String,
    @SerialName("data") val data: String
)
```

#### Domain Model
```kotlin
data class FeatureModel(
    val id: String,
    val data: String
)
```

### 3. Mapper
```kotlin
object FeatureMapper {
    fun ApiModel.toDomain(): DomainModel = DomainModel(
        id = id,
        data = data
    )
}
```

## Local Persistence

### Room Database

#### 1. Entity
```kotlin
@Entity(tableName = "feature_table")
data class FeatureEntity(
    @PrimaryKey val id: String,
    val data: String
)
```

#### 2. DAO
```kotlin
@Dao
interface FeatureDao {
    @Query("SELECT * FROM feature_table")
    fun getAll(): Flow<List<FeatureEntity>>
    
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(entity: FeatureEntity)
}
```

### SharedPreferences

Use DataStore instead of SharedPreferences for new code:

```kotlin
private val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "settings")

val exampleFlow: Flow<String> = dataStore.data
    .map { preferences -> 
        preferences[stringPreferencesKey("example")] ?: ""
    }
```

## Repository Pattern

### 1. Repository Interface (Domain Layer)
```kotlin
interface FeatureRepository {
    fun getData(): Flow<List<FeatureModel>>
    suspend fun refreshData()
}
```

### 2. Repository Implementation (Data Layer)
```kotlin
class FeatureRepositoryImpl @Inject constructor(
    private val api: FeatureApi,
    private val dao: FeatureDao
) : FeatureRepository {
    override fun getData(): Flow<List<FeatureModel>> =
        dao.getAll().map { entities ->
            entities.map { it.toDomain() }
        }

    override suspend fun refreshData() {
        try {
            val response = api.getData()
            if (response.isSuccessful) {
                response.body()?.let { apiModel ->
                    dao.insert(apiModel.toEntity())
                }
            }
        } catch (e: Exception) {
            throw e
        }
    }
}
```

## Best Practices

### Error Handling
- Use Result class for error handling
- Create custom error types
- Handle network errors appropriately
- Provide meaningful error messages

### Threading
- Use Dispatchers.IO for disk/network operations
- Use withContext for dispatcher switching
- Avoid blocking operations on main thread

### Caching
- Implement proper caching strategies
- Handle cache invalidation
- Use Room as single source of truth

### Testing
- Write unit tests for repositories
- Mock network responses
- Test error scenarios
- Verify mapping logic
