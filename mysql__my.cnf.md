[mysqld]
# ------------------------------------------
# GENERAL SETTINGS
# ------------------------------------------
innodb_buffer_pool_size = 8G        # It's recommended to allocate 60-70% of RAM; this is great for high-traffic systems
innodb_buffer_pool_instances = 8    # Create one instance for every 1GB; increase this if the buffer pool exceeds 1GB
innodb_log_file_size = 2G           # This stores transaction logs; 2GB is suitable for heavy workloads
innodb_log_buffer_size = 256M       # Transaction log buffer, helps manage write transactions more efficiently
innodb_flush_log_at_trx_commit = 1  # Set this to 1 to ensure all transactions are safe; set to 2 for better performance if data loss is acceptable

# ------------------------------------------
# PERFORMANCE TUNING
# ------------------------------------------
max_connections = 500               # Number of simultaneous connections; adjust based on your needs
thread_cache_size = 100             # Cache size for managing temporary connections; higher values are better under heavy load
table_open_cache = 4000             # Number of tables to keep open simultaneously; increase for larger databases
tmp_table_size = 512M               # Increase temporary table size to minimize disk access
max_heap_table_size = 512M          # Similar to tmp_table_size for in-memory tables, helps with speed

# ------------------------------------------
# CACHE SETTINGS
# ------------------------------------------
query_cache_type = 0                # Disable query cache as it can be detrimental under heavy load
query_cache_size = 0                # Set to zero to avoid caching at all

# ------------------------------------------
# INNODB SETTINGS
# ------------------------------------------
innodb_flush_method = O_DIRECT      # Avoid double caching of data in the OS, especially beneficial for SSDs
innodb_file_per_table = 1           # Each table is stored in a separate file for easier disk management
innodb_read_io_threads = 16         # Increase read I/O threads based on disk performance
innodb_write_io_threads = 16        # Increase write I/O threads
innodb_thread_concurrency = 0       # Let InnoDB manage thread concurrency automatically
innodb_io_capacity = 2000           # Set based on disk I/O capacity; adjust according to hardware
innodb_io_capacity_max = 4000       # Maximum IOPS for enhanced speed

# ------------------------------------------
# LOGGING & MONITORING
# ------------------------------------------
slow_query_log = 1                  # Enable slow query log to identify and optimize slow queries
slow_query_log_file = /var/log/mysql/slow.log  # Location for slow query log
long_query_time = 1                 # Log queries that take longer than 1 second
log_queries_not_using_indexes = 1   # Log queries that do not use indexes for optimization insights
log_error = /var/log/mysql/error.log  # Error log location
log_warnings = 2                    # Increase warning level for easier problem identification
general_log = 0                     # Disable general log to conserve system resources
general_log_file = /var/log/mysql/general.log  # Location for general log if enabled

# ------------------------------------------
# TIMEOUT SETTINGS
# ------------------------------------------
wait_timeout = 600                  # Time to wait for inactive connections before closing them
interactive_timeout = 600           # Similar to wait_timeout for interactive connections
net_read_timeout = 30               # Time to wait for reading data from connection
net_write_timeout = 30              # Time to wait for writing data to connection
lock_wait_timeout = 300             # Time to wait for locks before timing out

# ------------------------------------------
# CHARACTER SET
# ------------------------------------------
character-set-server = utf8mb4      # Enable UTF-8 support for special characters (e.g., emojis)
collation-server = utf8mb4_unicode_ci  # Use this collation for proper comparisons

# ------------------------------------------
# CONNECTION SETTINGS
# ------------------------------------------
max_connect_errors = 100000         # Number of allowed connection errors before blocking; higher values prevent unnecessary blocking

# ------------------------------------------
# BUFFER SETTINGS
# ------------------------------------------
key_buffer_size = 256M              # For MyISAM tables; not needed if using InnoDB
sort_buffer_size = 4M               # Sort buffer for ORDER BY queries
read_buffer_size = 4M               # Read buffer for table scans
read_rnd_buffer_size = 8M           # Read buffer for random scans
join_buffer_size = 8M               # Buffer for complex JOIN operations

# ------------------------------------------
# OTHER SETTINGS
# ------------------------------------------
table_definition_cache = 2000       # Number of table definitions to cache in MySQL
open_files_limit = 65535            # Limit on the number of open files the OS can manage









[mysqld]
# ------------------------------------------
# GENERAL SETTINGS
# ------------------------------------------
innodb_buffer_pool_size = 8G        # بهتره 60-70 درصد از RAM رو بهش بدی، واسه سیستم‌های پرترافیک خیلی خوبه
innodb_buffer_pool_instances = 8    # به ازای هر 1GB یه instance درست کن، اگه buffer pool از 1GB بیشتره، این عددو ببر بالا
innodb_log_file_size = 2G           # این لاگ تراکنش‌ها رو نگه می‌داره، واسه حجم کارای سنگین 2G خوبه
innodb_log_buffer_size = 256M       # بافر لاگ تراکنش‌ها، با این مقدار تراکنش‌های نوشتنی رو راحت‌تر می‌کنه
innodb_flush_log_at_trx_commit = 1  # این مقدار رو بذار 1 تا تضمین کنه همه تراکنش‌ها امن هستن، اما اگه کارایی بیشتر می‌خوای و نگران از دست رفتن داده‌ها نیستی می‌تونی بذاری 2

# ------------------------------------------
# PERFORMANCE TUNING
# ------------------------------------------
max_connections = 500               # تعداد کانکشن‌های همزمان، بسته به نیازت می‌تونی اینو بیشتر یا کمتر کنی
thread_cache_size = 100             # سایز کش نخ‌ها برای مدیریت کانکشن‌های موقتی، هر چی بالاتر بهتره توی بار سنگین
table_open_cache = 4000             # تعداد جداولی که همزمان باز نگه می‌داره، هر چی دیتابیس بزرگتر، اینو ببر بالا
tmp_table_size = 512M               # سایز جداول موقتی رو زیاد کن تا کمتر به دیسک مراجعه کنه
max_heap_table_size = 512M          # مشابه tmp_table_size برای جداول حافظه‌ای که به سرعت کمک می‌کنه

# ------------------------------------------
# CACHE SETTINGS
# ------------------------------------------
query_cache_type = 0                # کش کوئری رو غیرفعال کن چون توی بار سنگین معمولاً ضرر داره
query_cache_size = 0                # سایز کش کوئری صفره تا اصلاً کش نکنه

# ------------------------------------------
# INNODB SETTINGS
# ------------------------------------------
innodb_flush_method = O_DIRECT      # واسه جلوگیری از کش شدن دوباره داده‌ها توی سیستم‌عامل، مخصوصاً واسه SSDها خیلی خوبه
innodb_file_per_table = 1           # هر جدول توی یه فایل جداگانه ذخیره می‌شه که مدیریت دیسک رو راحت‌تر می‌کنه
innodb_read_io_threads = 16         # نخ‌های ورودی/خروجی برای خوندن رو بسته به دیسکت زیاد کن
innodb_write_io_threads = 16        # نخ‌های ورودی/خروجی برای نوشتن
innodb_thread_concurrency = 0       # بذار InnoDB خودش همزمانی نخ‌ها رو مدیریت کنه
innodb_io_capacity = 2000           # ظرفیت IOPS دیسک، بسته به دیسک و سخت‌افزارت این عددو تنظیم کن
innodb_io_capacity_max = 4000       # مقدار ماکسیمم IOPS برای سرعت بیشتر

# ------------------------------------------
# LOGGING & MONITORING
# ------------------------------------------
slow_query_log = 1                  # لاگ کوئری‌های کند رو فعال کن که بتونی شناسایی و بهینه‌سازی کنی
slow_query_log_file = /var/log/mysql/slow.log  # محل ذخیره لاگ کوئری‌های کند
long_query_time = 1                 # کوئری‌هایی که بیش از 1 ثانیه طول می‌کشن لاگ می‌شن
log_queries_not_using_indexes = 1   # کوئری‌هایی که از ایندکس استفاده نمی‌کنن رو لاگ کن تا ببینی کجاها نیاز به بهینه‌سازی داری
log_error = /var/log/mysql/error.log  # محل ذخیره خطاهای MySQL
log_warnings = 2                    # سطح هشدارها رو زیاد کن تا راحت‌تر مشکلات رو پیدا کنی
general_log = 0                     # لاگ عمومی رو خاموش کن که منابع سیستمت رو زیاد مصرف نکنه
general_log_file = /var/log/mysql/general.log  # محل ذخیره لاگ عمومی در صورت فعال بودن

# ------------------------------------------
# TIMEOUT SETTINGS
# ------------------------------------------
wait_timeout = 600                  # زمان انتظار برای کانکشن‌های غیرفعال، بعد این زمان قطع می‌شه
interactive_timeout = 600           # مشابه wait_timeout برای کانکشن‌های تعاملی
net_read_timeout = 30               # زمان انتظار برای خوندن داده‌ها از کانکشن
net_write_timeout = 30              # زمان انتظار برای نوشتن داده‌ها به کانکشن
lock_wait_timeout = 300             # زمان انتظار واسه قفل شدن قبل از اینکه قطع بشه

# ------------------------------------------
# CHARACTER SET
# ------------------------------------------
character-set-server = utf8mb4      # UTF-8 رو برای پشتیبانی از کاراکترهای خاص (مثل اموجی‌ها) فعال کن
collation-server = utf8mb4_unicode_ci  # این collation رو بذار که مقایسه درست انجام شه

# ------------------------------------------
# CONNECTION SETTINGS
# ------------------------------------------
max_connect_errors = 100000         # تعداد خطاهای مجاز قبل از بلاک شدن کانکشن، مقدار بالاتر کمک می‌کنه که کانکشن‌ها بی‌دلیل بلاک نشن

# ------------------------------------------
# BUFFER SETTINGS
# ------------------------------------------
key_buffer_size = 256M              # برای جداول MyISAM، اگه از InnoDB استفاده می‌کنی، نیاز نیست بالا باشه
sort_buffer_size = 4M               # بافر مرتب‌سازی برای کوئری‌های ORDER BY
read_buffer_size = 4M               # بافر خوندن برای اسکن جداول
read_rnd_buffer_size = 8M           # بافر خوندن تصادفی برای اسکن‌های رندوم
join_buffer_size = 8M               # بافر برای انجام JOIN های پیچیده

# ------------------------------------------
# OTHER SETTINGS
# ------------------------------------------
table_definition_cache = 2000       # تعداد تعریف جدول‌هایی که MySQL تو کش نگه می‌داره
open_files_limit = 65535            # تعداد فایل‌های باز که سیستم‌عامل می‌تونه مدیریت کنه
