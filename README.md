### Summary

---

[inhaconnet.mov](attachment:e68576e3-eeb1-47c1-b71d-b5673f734c2b:inhaconnet.mov)

### Inha Connect

---

Inha Connect, Timetable-Based Group Formation Service (Mar. 2023 - Dec. 2024)
forms groups of students with similar class schedules, operating for 2 years with recruitment every
semester.

- Sole Product Developer(Planning, Design, Development, Promotion)
- Tech Stack: CleanShotX, Google Forms, GPT-4, C++, Vercel
- Matching Algorithm: https://lrl.kr/mCgm
- Live Website: [https://www.inha.or.kr](https://www.inha.or.kr/)
- Matched 324 students.
- Quickly built a landing page using templates.
- Collected timetables via Google Forms and processed them using OCR.
- Crawled Everytime (Korean class schedule platform) to verify actual class times.
- Applied custom weights considering course codes and overlapping class sections.
- Conducted matching using GPT-4 with anonymized xlsx data.
- Minimized development costs by leveraging no-code solutions.

### Matching Algorithm

---

[ctp.mov](attachment:741d7623-6168-4f80-a0f4-97cfdf7eeb87:ctp.mov)

```cpp
#include <algorithm>
#include <iostream>
#include <map>
#include <string>
#include <vector>
using namespace std;
const int MAXN = 201;
const int INF = 0x3f3f3f3f;
#define f first
#define s second
typedef pair<string, vector<string>> psv;
typedef pair<int, int> ii;

string course;
int idx = 1;
int num;
int total_sum = 0;
vector<psv> v;
int cst[MAXN][MAXN] = {
    0,
};
bool vst[MAXN] = {
    0,
};
map<string, int> gpa;
string name[MAXN];

int compare(string a, string b) {
  
  string course_number = a.substr(0, 7); 
  if (a == b) return gpa[course_number]; // 학수번호+분반이 같을경우
  else if (a.substr(0, 7) == b.substr(0, 7)) return 2; // 학수번호가 같을경우
  else return 0; // 불일치
}

bool sorting(pair<int, int> p, pair<int, int> p2) {
  if (p.first == p2.first)
    return p.second < p2.second;
  return p.first > p2.first;
}

int main() {
  // 학수번호와 그에 해당하는 학점
  int m; cin >> m;
  while (m--) {
    cin >> course >> num;
    gpa[course] = num;
  }
  int n; cin >> n;
  for (int i = 0; i < n; i++) {
    vector<string> copy;
    cin >> course >> num;
    name[i] = course;
    while (num--) {
      string course;
      cin >> course;
      copy.push_back(course);
    }
    v.push_back({course, copy});
  }

  for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
      for (auto &p : v[i].s)
        for (auto &q : v[j].s)
          cst[i][j] += compare(p, q);

  vector<vector<ii>> bd;
  for (int i = 0; i < n; i++) {
    vector<ii> copy;
    for (int j = 0; j < n; j++)
      if (j != i)
        copy.push_back({cst[i][j], j});
    sort(copy.begin(), copy.end(), sorting);
    bd.push_back(copy);
  }
  for (int i = n; i >= 5; i -= 5) {
    ii save = {-INF, -INF}; // cst, line idx
    for (int j = 0; j < n; j++) {
      if (vst[j]) continue;
      int sum = 0, cnt = 4;
      for (int k = 0; k < n - 1; k++) {
        if (cnt == 0) break;
        if (vst[k]) continue;
        sum += cst[j][bd[j][k].s];
        cnt--;
      }
      if (sum > save.f) save = {sum, j};
    }
    vst[save.s] = 1;
    cout << idx << "조 / "
         << "유사도 : " << save.f << "\n";
    idx++;
    cout << name[save.s] << "\n";
    int cnt = 4;
    for (int k = 0; k < n - 1; k++) {
      int ans = save.s;
      if (cnt == 0) break;
      if (vst[bd[ans][k].s]) continue;
      vst[bd[ans][k].s] = 1;
      cnt--;
      cout << name[bd[ans][k].s] << "\n";
    }
    total_sum += save.f;
    cout << "\n";
  }
  cout << idx << "조 / "
       << "유사도 : " << 0 << "\n";
  for (int i = 0; i < n; i++) {
    if (!vst[i]) cout << name[i] << "\n";
  }

  return 0;
}
```

- It was a great experience to break down an abstract real-world problem like a problem-solving (PS) challenge and find a solution.

![CleanShot 2025-04-18 at 08.34.20@2x.png](attachment:e9e7df10-5871-4fc4-b349-8e999f7429d3:CleanShot_2025-04-18_at_08.34.202x.png)

- This is the list I created for time-based group formation. (More to come!)

![yeah.png](attachment:93358e1f-1091-4495-b1a1-117fd61089bf:yeah.png)

- After matching, I used Figma to visualize a relationship diagram, allowing participants to easily view their compatibility rates within the communication space.
