*// !!!!将由 tree 对象 t 描述的弧线段描绘为一条曲线* 

void

concater_rep::typeset_arc (tree t, path ip, bool close) {

BEGIN_MAGNIFY

  int i, n= N(t);

  array<point> a(n+2);*// 存储由 tree 对象描述的路径上的一组点。* 

  *// 获得上述的点*

  for (i=0; i<n; i++)

​    a[i]= env->as_point (env->exec (t[i]));

  if (N(a) == 0 || N(a[0]) == 0) typeset_error (t, ip);

  else {

​      if (n == 2) {

​        cout << "已有两个点，n为:" << n << LF;

​        point center = a[0];

​        point x = a[1];

​        point y(n), z(n);

​        long double r = sqrt(pow(center[0] - x[0], 2) + pow(center[1] -x[1], 2));

​        cout << "半径:" << r << LF;

​        y[0] = center[0] + r;

​        y[1] = center[1];

​        z[0] = center[0] - r;

​        z[1] = center[1];

​        cout << "两点计算完成,坐标为：" << x << y << z << LF;

​        if(y == x){

​          y[0] = center[0];

​          y[1] = center[1] + r ;

​          cout << "x和y坐标相同，修改y后坐标：" << x << y << z << LF;

​        } else if(z == x) {

​          z[0] = center[0];

​          z[1] = center[1] + r;

​          cout << "x和z坐标相同，修改z后坐标：" << x << y << z << LF;

​        }

​        cout << "三点坐标已全" << LF;

​        a[2] = y;

​        a[3] = z;

​        cout << "圆心：" << a[0] << "x:"<< a[1] << "y:" << a[2] << "z:" << a[3] << LF;

​        array<point> b(3);

​        for(int i = 0; i < 3; i++){

​          b[i] = a[i+1];

​          cout << b[i] << LF;

​        }

​        *// 存储曲线上每个点对应的源文档树路径，ip 是路径*

​        array<path> cip(3);

​        for (i=0; i<3; i++)

​          cip[i]= descend (ip, i);

​        n+1;

​        curve c= env->fr (arc (b, cip, close));

​        adjust_extremities (c, env->white_zones);

​        print (curve_box (ip, c, env->line_portion, env->pen,

​                          env->dash_style, env->dash_motif, env->dash_style_unit,

​                          env->fill_brush, typeset_line_arrows (ip)));

​    } else {

​      cout << "tree:" << t << LF;

​      *//cout << "tree_rep:" << t.rep->ref_count << LF;*

​      *//t.rep->ref_count++;*

​      tree_rep t;

​      t.tree_label op = CARC;

​      typeset_line (t, ip, close);

​    }

  }

END_MAGNIFY

}

```C++

/******************************************************************************
 * MODULE     : tree_test.cpp
 * DESCRIPTION: Tests on tree
 * COPYRIGHT  : (C) 2019-2021  Darcy Shen
 *******************************************************************************
 * This software falls under the GNU general public license version 3 or later.
 * It comes WITHOUT ANY WARRANTY WHATSOEVER. For details, see the file LICENSE
 * in the root directory or <http://www.gnu.org/licenses/gpl-3.0.html>.
 ******************************************************************************/

#include "tree.hpp"
#include <QtTest/QtTest>

class TestTree : public QObject {
  Q_OBJECT

private slots:
  void test_is_atomic ();
  void test_is_tuple ();
  void test_is_concat ();
  void test_is_compound ();
  void test_is_bool ();
  void test_is_int ();
  void test_is_func ();
  void test_is_double ();
  void test_is_string ();
  void test_is_generic ();
  void test_get_label ();
  void test_arity ();
};

void
TestTree::test_is_atomic () {
  QVERIFY (is_atomic (tree ()));
}

void
TestTree::test_is_tuple () {
  QVERIFY (is_tuple (tuple ()));
  QVERIFY (is_tuple (tuple (tree ())));
  QVERIFY (is_tuple (tuple (tree (), tree ())));
  QVERIFY (is_tuple (tuple (tree (), tree (), tree ())));
  QVERIFY (is_tuple (tuple (tree (), tree (), tree (), tree ())));
  QVERIFY (is_tuple (tuple (tree (), tree (), tree (), tree (), tree ())));
}

void
TestTree::test_is_concat () {
  QVERIFY (is_concat (concat ()));
  QVERIFY (is_concat (concat (tree ())));
  QVERIFY (is_concat (concat (tree (), tree ())));
  QVERIFY (is_concat (concat (tree (), tree (), tree ())));
  QVERIFY (is_concat (concat (tree (), tree (), tree (), tree ())));
  QVERIFY (is_concat (concat (tree (), tree (), tree (), tree (), tree ())));
}

void
TestTree::test_is_compound () {
  QVERIFY (is_compound (tree (FRAC, 3)));
  QVERIFY (!is_compound (tree ()));
}

void
TestTree::test_is_bool () {
  QVERIFY (is_bool (tree ("true")));
  QVERIFY (is_bool (tree ("false")));
  QVERIFY (!is_bool (tree ("other")));
}

void
TestTree::test_is_int () {
  QVERIFY (!is_int (tree ()));
  QVERIFY (is_int (tree ("+12")));
  QVERIFY (is_int (tree ("-12")));
  QVERIFY (!is_int (tree ("-+12")));
  QVERIFY (!is_int (tree ("one")));
}

void
TestTree::test_is_func () {
  QVERIFY (!is_func (tree (FRAC, 3),tree_label(DOCUMENT)));
  QVERIFY (!is_func (tree (),tree_label()));
  QVERIFY (is_func (tree (LSUP,4),tree_label(LSUP)));
}

void
TestTree::test_is_double () {
  QVERIFY (!is_double (tree ()));
  QVERIFY (is_double (tree ("3.15")));
  QVERIFY (!is_double (tree ("0")));
  QVERIFY (is_double (tree ("0.0")));
  QVERIFY (is_double (tree ("3.1415926")));
  QVERIFY (is_double (tree ("-3.1415926")));
  QVERIFY (!is_double (tree ("nothing")));
}

void
TestTree::test_is_string () {
  QVERIFY (is_string (tree ()));
  QVERIFY (!is_string (tree ("0")));
  QVERIFY (is_string (tree ("+121234")));
  QVERIFY (is_string (tree ("seigj")));
  QVERIFY (!is_string (tree ("!@#$$!")));
  QVERIFY (!is_string (tree (FRAC, 3)));
}

void
TestTree::test_is_generic () {
  QVERIFY (!is_generic (tree (RIGID,0)));
  QVERIFY (is_generic (tree (PARA,-1)));
  QVERIFY (!is_generic (tree (RIGID,1234)));
  QVERIFY (is_generic (tree (IF_PAGE_BREAK,-45)));
  QVERIFY (!is_generic (tree (RIGID,2)));
}

void
TestTree::test_get_label () {
  QVERIFY (get_label (tree (RIGID,1)) == string("RIGID"));
  QVERIFY (get_label (tree (ARC,0)) != string("RIGID"));
  QVERIFY (get_label (tree (ARC,-1)) == string("ARC"));
}

void
TestTree::test_arity () {
  QVERIFY (!arity (tree (STRING, -1)));
  QVERIFY (arity (tree (ARC, 0)));
  QVERIFY (arity (tree (RIGID,3)));
  QVERIFY (!arity (tree (STRING,5)));
}

QTEST_MAIN (TestTree)
#include "tree_test.moc"

```

```
// !!!!将由 tree 对象 t 描述的弧线段描绘为一条曲线* 
void
concater_rep::typeset_arc (tree t, path ip, bool close) {
BEGIN_MAGNIFY
  int i, n= N(t);
  // 获得 tree 对象内包含的一组 point ，放在数组a里。
  array<point> a(n+2);
  for (i=0; i<n; i++)
    a[i]= env->as_point (env->exec (t[i]));
  // 根据数组a中的 point 的个数，决定执行：
  if (N(a) == 0 || N(a[0]) == 0) 
    typeset_error (t, ip);
  else {
     if (n == 2) {
        // 两个点分别为圆心 centeer 和圆上一点 x
        point center = a[0];
        point x = a[1];
        point y(n), z(n);
        long double r = sqrt(pow(center[0] - x[0], 2) + pow(center[1] -x[1], 2));
        cout << "半径:" << r << LF;
        y[0] = center[0] + r;
        y[1] = center[1];
        z[0] = center[0] - r;
        z[1] = center[1];
        cout << "两点计算完成,坐标为：" << x << y << z << LF;
        if(y == x){
          y[0] = center[0];
          y[1] = center[1]// 存储由 tree 对象描述的路径上的一组点。 + r ;
          cout << "x和y坐标相同，修改y后坐标：" << x << y << z << LF;
        } else if(z == x) {
          z[0] = center[0];
          z[1] = center[1] + r;
          cout << "x和z坐标相同，修改z后坐标：" << x << y << z << LF;
       }
        cout << "三点坐标已全" << LF;
        a[2] = y;
        a[3] = z;
       cout << "圆心：" << a[0] << "x:"<< a[1] << "y:" << a[2] << "z:" << a[3] << LF;
       array<point> b(3);
      for(int i = 0; i < 3; i++){
         b[i] = a[i+1];
         cout << b[i] << LF;
      }
       // 存储曲线上每个点对应的源文档树路径，ip 是路径
       array<path> cip(3);
       for (i=0; i<3; i++)
         cip[i]= descend (ip, i);
       n+1;
       curve c= env->fr (arc (b, cip, close));
      adjust_extremities (c, env->white_zones);
      print (curve_box (ip, c, env->line_portion, env->pen,
                      env->dash_style, env->dash_motif, env->dash_style_unit,
                     env->fill_brush, typeset_line_arrows (ip)));
  } //else {
//     cout << "tree:" << t << LF;
//      //cout << "tree_rep:" << t.rep->ref_count << LF;
//      //t.rep->ref_count++;
//      //tree_rep t;
//      //t.tree_label op = CARC;
//      typeset_line (t, ip, close);
//    }
  }
END_MAGNIFY
}
```

enx->fr 需要参数 ------ arc（）函数返回值作为参数 -------- arc（）函数需要point数组，path数组，是否闭合三个参数

arc（）调用tm_new<arc_rep>模板函数，将其返回值返回 —— tm_new<arc_rep>返回一个arc_rep类型的指针

arc_rep这个类包含point ，path等成员。它的一个构造函数需要point数组，path数组，是否闭合。

虚拟一个arc_rep类就可以创造一个曲线
