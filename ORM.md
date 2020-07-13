# ORM

*sql*

```sql
-- mysql
CREATE TABLE books (
  `id`          BIGINT(20)     NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `insert_time` TIMESTAMP      NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` TIMESTAMP      NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  `title`       VARCHAR(255)   NOT NULL COMMENT '书本名称',
  `author_id`   INT            NOT NULL COMMENT '作者id',
  `subject_id`  INT            NOT NULL COMMENT '书本类别id',

  PRIMARY KEY (`id`),
  KEY `idx_title`(`title`),
  KEY `idx_author_id`(`author_id`),
  KEY `idx_subject_id`(`subject_id`),
  KEY `idx_insert_time` (`insert_time`),
  KEY `idx_update_time` (`update_time`)
)
  ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4
  COLLATE = utf8mb4_unicode_ci
  COMMENT = '书本记录表';
```

+ *版本*

  + (python) sqlalchemy==1.3.18, pydantic==1.4
  + (go) upper.io/db.v3 v3.7.1
  + java

  ````xml
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.2.7.RELEASE</version>
      <relativePath/> <!-- lookup parent from repository -->
  </parent>
  
  <dependencies>
      <!--        web starter with logback exclusion-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
          <exclusions>
              <exclusion>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-logging</artifactId>
              </exclusion>
          </exclusions>
      </dependency>
  
      <!--        logger-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-log4j2</artifactId>
      </dependency>
  
      <!--        data class-->
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.8</version>
          <scope>provided</scope>
      </dependency>
  
      <!--        json serializer-->
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>fastjson</artifactId>
          <version>1.2.59</version>
      </dependency>
  
      <!--        db-->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <scope>runtime</scope>
      </dependency>
  
      <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>2.1.2</version>
      </dependency>
  
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatis-plus-boot-starter</artifactId>
          <version>3.3.1.tmp</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-jpa</artifactId>
      </dependency>
  
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid-spring-boot-starter</artifactId>
          <version>1.1.17</version>
      </dependency>
  
  
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-configuration-processor</artifactId>
          <optional>true</optional>
      </dependency>
  
      <!--        test-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <scope>test</scope>
          <exclusions>
              <exclusion>
                  <groupId>org.junit.vintage</groupId>
                  <artifactId>junit-vintage-engine</artifactId>
              </exclusion>
          </exclusions>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <scope>test</scope>
      </dependency>
  </dependencies> 
  ````

  

## Model如何定义

+ go

```go
type Book struct {
	// upper-db will convert Go types into database-specific types and vice
	// versa, as shown in the "books" table on the left.
	ID         uint      `db:"id,omitempty"`
    InsertTime time.Time `db:"insert_time,omitempty"`
	UpdateTime time.Time `db:"update_time,omitempty"`
	Title      string    `db:"title" json:"title"`
	AuthorID   uint      `db:"author_id" json:"author_id"`
	SubjectID  uint      `db:"subject_id" json:"subject_id"`
}
```

+ python

````python
# -*- coding: UTF-8 -*-

from datetime import datetime

from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, DateTime

from pydantic import BaseModel

Base = declarative_base()


class Book(Base):
    __tablename__ = 'books'

    id = Column(Integer, primary_key=True)
    insert_time = Column(DateTime, default=datetime.now)
    update_time = Column(DateTime, default=datetime.now, onupdate=datetime.now)
    title = Column(String(255), nullable=False) # use of TEXT is preferred
    author_id = Column(Integer, nullable=False)
    subject_id = Column(Integer, nullable=False)
    

 class BookDTO(BaseModel):
    id: int
    insert_time: datetime
    update_time: datetime
    title: str
    author_id: int
    subject_id: int

    class Config:
        orm_mode = True
````

+ java

````java
//JPA, ignore package info here

import lombok.Data;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import java.sql.Timestamp;

@Data
@Entity
@Table(name = "books")
public class Book {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  private String title;

  @Column(name = "author_id")
  private Long authorId;

  @Column(name = "subject_id")
  private Long subjectId;

  @Column(
      name = "insert_time",
      insertable = false,
      updatable = false,
      columnDefinition = "TIMESTAMP DEFAULT CURRENT_TIMESTAMP")
  private Timestamp insertTime;

  @Column(
      name = "update_time",
      insertable = false,
      updatable = false,
      columnDefinition = "TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP")
  private Timestamp updateTime;
}
````

````java
// mybatis-plus, ignore package info here
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;

import java.sql.Timestamp;

@Data
@TableName("books")
public class Book {

  @TableId(type = IdType.AUTO)
  private Long id;

  //  @TableField("insert_time") // default infer name is insert_time
  private Timestamp insertTime;
  private Timestamp updateTime;

  private String title;
  private Long authorId;
  private Long subjectId;
}
````

## 连接池



## Session如何控制，是否是线程安全

+ **python**

在Sqlalchemy中Session有以下特点：

1. 创建和管理与数据库的会话
2. 维持对所加载对象的操作（内部维护了一个Identity Map）
3. 非线程安全
4. （建议）在一次会话结束后对当前Session进行销毁
5. 在多线程环境使用scoped_session对SessionFactory进行封装，使得SessionFactory在不同的thread context下才会返回不同的Session。对同一线程，手动触发sess.remove后才会更新该线程下的session。

ref to:

+ [SessionBasics](https://docs.sqlalchemy.org/en/13/orm/session_basics.html)
+ [FAQ](https://docs.sqlalchemy.org/en/13/faq/index.html)

````python
from contextlib import contextmanager

from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, Session, scoped_session

# create engine
some_engine = create_engine('mysql://user:password@host:port/db?charset=utf8')
# create Session factory
SessionFactory = sessionmaker(bind=some_engine)
# create thread local Session factory
ThreadLocalSessFactory = scoped_session(SessionFactory)

@contextmanager
def session_scope():
    """Provide a transactional scope around a series of operations."""
    session = SessionFactory()
    try:
        yield session
        session.commit()
    except:
        session.rollback()
        raise
    finally:
        session.close()

def doThingWithSessionOne(sess: Session):
    print(id(sess))


def doThingsWithSessionTwo(sess: Session):
    print(id(sess))


if __name__ == '__main__':
    with session_scope() as sess:
        doThingWithSessionOne(sess)  # id: 140131068932448
    with session_scope() as sess:
        doThingsWithSessionTwo(sess) # id: 140131068932840
````

## 如何做CRUD

+ python
  + [Querying](https://docs.sqlalchemy.org/en/13/orm/tutorial.html#querying)
  + [QueryApi](https://docs.sqlalchemy.org/en/13/orm/query.html)

````python
class BookRepository:

    ## create
    def insert_book(self, sess: Session, b: Book) -> int:
        sess.add(b)
        sess.commit()
        return b.id

    # query
    def select_book_by_id(self, sess: Session, id: int) -> BookDTO:
        b = sess.query(Book).filter_by(id=id).one()  # raise Exception if no record found
        return BookDTO.from_orm(b)

    # modify if exists
    def modify_book_by_id(self, sess: Session, id: int, b: BookDTO):
        sess.query(Book).filter_by(id=id).update(b.dict(skip_defaults=True))

    # delete if exists
    def delete_book_by_id(self, sess: Session, id: int):
        sess.query(Book).filter_by(id=id).delete()
````

+ go
  + [gorm](https://github.com/go-gorm/gorm)
  + [xorm](https://gitea.com/xorm/xorm)
  + [upper db](https://github.com/upper/db)

```go
// upper_db sample
package v1

import (
	"log"

	"upper.io/db.v3/lib/sqlbuilder"
	"upper.io/db.v3/mysql"
)

var dbSettings = mysql.ConnectionURL{
	Database: `db_name`,
    Host:     `host:port`,
	User:     `user`,
	Password: `password`,
}

var sessMysql sqlbuilder.Database

func init() {
	var err error
	sessMysql, err = mysql.Open(dbSettings)
	if err != nil {
		log.Fatal("Open: ", err)
	}
}

// IBookRepository - book repository interface
type IBookRepository interface {
	InsertBook(b Book)
}

//Repository ... represent CRUD on table
type Repository struct {
	tableName string
}

//RepositoryFactory ... factory for Repository instance creation
func RepositoryFactory(tableName string) Repository {
	return Repository{tableName: tableName}
}

// InsertBook ...insert a book instance to table
func (br Repository) InsertBook(b Book) (interface{}, error) {
	t := sessMysql.Collection(br.tableName)
	return t.Insert(b)
}

// CleanDb ...close db sessions when application exists
func CleanDb() {
	err := sessMysql.Close()
	if err != nil {
		log.Println(err)
	}
}
```

+ java(// TODO: 7/13/20, 加入dataSource、SqlSessionFactory、SqlSession的说明)

````java
// mybatis-plus, ignore package info here
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Component;

import com.demo.domin.entity.Book;

@Mapper
@Component
public interface BookMapper extends BaseMapper<Book> {}
````

```java
// jpa, ignore package info here
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

import com.demo.domin.entity.Book;

@Repository
public interface BookRepository extends CrudRepository<Books, Long> {}
```



## 事务

## 分页

## 如何做分库或分表

## 如何做关联查询

