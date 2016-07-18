Learned about ANOVA - very useful.  Successive models must be nested for ANOVA.

## 7.Q.1 - what?

[Question 7.Q.1](https://lagunita.stanford.edu/courses/HumanitiesSciences/StatLearning/Winter2016/courseware/43d59889973b4b34a7070918f2a7bb3f/0fa2d1b669d64d4388109a95591250dc/)

I really don't understand this distinction about basis of fit, e.g. the
difference between fit and fit2 (from ch7 lab):

```r
> fit=lm(wage~poly(age,4),data=Wage)
> fit2=lm(wage~poly(age,4,raw=T),data=Wage)
```

## 7.6.a

CV:

```r
library(ISLR)
library(boot)
cv.err=rep(0,10)
for(i in 1:10) {
    fit = glm(wage~poly(age,i),data=Wage)
    cv.err[i] = cv.glm(Wage, fit, K=10)$delta[2]
}
plot(cv.err, ylim=c(1580,1700))
lines(cv.err)
min.err = min(cv.err)
sd.err = sd(cv.err)
# se.err = sd.err / sqrt(10) # See question below
abline(h=min.err+0.2*sd.err,col="red",lty=2)
abline(h=min.err-0.2*sd.err,col="red",lty=2)
```

Why does http://blog.princehonest.com/stat-learning/ch7/6.html plot 0.2-stdev
lines to select min d?  I thought it was +/- 1 stderr, which would be
stdev/sqrt(N) = 0.316.... ???


ANOVA:

```r
fit.1 = lm(wage~poly(age,1),data=Wage)
fit.2 = lm(wage~poly(age,2),data=Wage)
fit.3 = lm(wage~poly(age,3),data=Wage)
fit.4 = lm(wage~poly(age,4),data=Wage)
fit.5 = lm(wage~poly(age,5),data=Wage)
fit.6 = lm(wage~poly(age,6),data=Wage)
fit.7 = lm(wage~poly(age,7),data=Wage)
fit.8 = lm(wage~poly(age,8),data=Wage)
fit.9 = lm(wage~poly(age,9),data=Wage)
fit.10 = lm(wage~poly(age,10),data=Wage)
anova(fit.1,fit.2 ,fit.3 ,fit.4 ,fit.5 ,fit.6 ,fit.7 ,fit.8 ,fit.9 ,fit.10)
```

anova(glm, glm, glm, ...) gives different statistics than anova(lm,lm,lm,...)
-- anova(glm...) gives Deviance and anova(lm...) gives F-stat and p-value.

```r
library(ISLR)
fit=lm(wage~poly(age,3), data=Wage)
agelims = range(Wage$age)
age.grid=seq(agelims[1],agelims[2])
preds=predict(fit,newdata=list(age=age.grid),se=T)
plot(Wage$age,Wage$wage)
lines(age.grid, preds$fit, col="red",lwd=2)
lines(age.grid, preds$fit+2*preds$se.fit, lty=2, col="red")
lines(age.grid, preds$fit-2*preds$se.fit, lty=2, col="red")
```

## 7.7

```r
all.cvs = rep(NA,10)
for (i in 2:10) {
  Wage$age.cut = cut(Wage$age, i)
  lm.fit = glm(wage~age.cut, data=Wage)
  all.cvs[i] = cv.glm(Wage, lm.fit, K=10)$delta[2]
}
plot(all.cvs, type="l")
```
