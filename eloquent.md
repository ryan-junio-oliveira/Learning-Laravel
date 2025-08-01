
# 📘 TUTORIAL DEFINITIVO: ELOQUENT MODELS NO LARAVEL

---

## 📌 SUMÁRIO

1. [Introdução ao Eloquent](#1)
2. [Criando Models](#2)
3. [Atributos Comuns](#3)
4. [Relacionamentos](#4)
5. [Mutators & Accessors](#5)
6. [Casting de Atributos](#6)
7. [Query Scopes](#7)
8. [Observers e Events](#8)
9. [Mass Assignment](#9)
10. [Soft Deletes](#10)
11. [Custom Collections e Casts](#11)
12. [Outros Recursos Avançados](#12)

---

<a name="1"></a>
## ✅ 1. Introdução ao Eloquent

Eloquent é o ORM oficial do Laravel. Cada model representa uma tabela no banco de dados.

---

<a name="2"></a>
## 🛠️ 2. Criando Models

```bash
php artisan make:model Post
php artisan make:model Post -mcr # cria também migration, controller e resource
```

---

<a name="3"></a>
## 🧱 3. Atributos Comuns

```php
class Post extends Model
{
    protected $table = 'posts';
    protected $primaryKey = 'id';
    public $timestamps = true;
    protected $fillable = ['title', 'content'];
    protected $hidden = ['password'];
    protected $casts = ['is_active' => 'boolean'];
}
```

---

<a name="4"></a>
## 🔗 4. Relacionamentos

```php
// One to One
public function profile()
{
    return $this->hasOne(Profile::class);
}

// One to Many
public function posts()
{
    return $this->hasMany(Post::class);
}

// Many to One
public function user()
{
    return $this->belongsTo(User::class);
}

// Many to Many
public function roles()
{
    return $this->belongsToMany(Role::class)->withTimestamps();
}

// Polimórficos
public function imageable()
{
    return $this->morphTo();
}
```

---

<a name="5"></a>
## 🧬 5. Mutators & Accessors

```php
// Accessor
public function getNameUpperAttribute()
{
    return strtoupper($this->name);
}

// Mutator
public function setNameAttribute($value)
{
    $this->attributes['name'] = strtolower($value);
}
```

---

<a name="6"></a>
## 🔁 6. Casting de Atributos

```php
protected $casts = [
    'email_verified_at' => 'datetime',
    'settings' => 'array',
    'active' => 'boolean',
];
```

---

<a name="7"></a>
## 🔍 7. Query Scopes

```php
public function scopeActive($query)
{
    return $query->where('active', 1);
}
```

Uso:
```php
User::active()->get();
```

---

<a name="8"></a>
## 🧠 8. Observers e Events

```bash
php artisan make:observer UserObserver --model=User
```

Métodos disponíveis:
- creating / created
- updating / updated
- saving / saved
- deleting / deleted

---

<a name="9"></a>
## 🛡️ 9. Mass Assignment

```php
protected $fillable = ['name', 'email'];
protected $guarded = ['admin'];
```

---

<a name="10"></a>
## 🧼 10. Soft Deletes

```php
use Illuminate\Database\Eloquent\SoftDeletes;

class Post extends Model
{
    use SoftDeletes;
    protected $dates = ['deleted_at'];
}
```

---

<a name="11"></a>
## 🔄 11. Custom Collections & Custom Casts

### Custom Collection:
```php
class CustomPostCollection extends Illuminate\Database\Eloquent\Collection
{
    public function published()
    {
        return $this->filter->isPublished();
    }
}

public function newCollection(array $models = [])
{
    return new CustomPostCollection($models);
}
```

### Custom Cast:
```php
class JsonCast implements CastsAttributes
{
    public function get($model, string $key, $value, array $attributes)
    {
        return json_decode($value);
    }

    public function set($model, string $key, $value, array $attributes)
    {
        return json_encode($value);
    }
}
```

---

<a name="12"></a>
## 🧰 12. Outros Recursos Avançados

- `touches`: atualiza timestamps de modelos relacionados
- `withDefault`: define valor padrão para `belongsTo`
- `appends`: força atributos customizados em JSON
- `replicate()`: clona registros
- `loadMissing()`: lazy eager load
- `firstOrCreate()`, `updateOrCreate()`

```php
User::firstOrCreate(['email' => $email], ['name' => $name]);
```

---

Este tutorial cobre a base e recursos avançados do Eloquent. Para exemplos práticos, scaffolding com factories, seeders e testes, posso gerar um projeto base Laravel com os recursos desejados.
