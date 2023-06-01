*// 将由 tree 对象 t 描述的弧线段描绘为一条曲线* 

void

concater_rep::typeset_arc (tree t, path ip, bool close) {

BEGIN_MAGNIFY

  int i, n= N(t);

  array<point> a(n);*// 存储由 tree 对象描述的路径上的一组点。* 

  *// 获得上述的点*

  for (i=0; i<n; i++)

​    a[i]= env->as_point (env->exec (t[i]));

  *// 存储曲线上每个点对应的源文档树路径，ip 是路径* 

  array<path> cip(n);

  for (i=0; i<n; i++)

​    cip[i]= descend (ip, i);

  if (N(a) == 0 || N(a[0]) == 0) typeset_error (t, ip);

  else {

​      if (n == 2) {

​        cout << "已有两个点，n为2" << LF;

​        *// 求两个点所连成的线段的中点（即圆心p）* 

​         

​        *// 求线段的斜率k1* 

 

​        *// 求垂直于该线段的斜率k2* 

 

​        *// 根据斜率k2和圆心k2创建直线* 

 

​        *// 求圆和直线的交点即为a[2]* 

 

​        *// 根据三点画出圆。* 

​        curve c= env->fr (arc (a, cip, close));

​        adjust_extremities (c, env->white_zones);

​        print (curve_box (ip, c, env->line_portion, env->pen,

​                          env->dash_style, env->dash_motif, env->dash_style_unit,

​                          env->fill_brush, typeset_line_arrows (ip)));

​    }

  }

END_MAGNIFY

}