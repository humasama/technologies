# Intro
- Hyper is a rust based http library (providing implementation for http1, http2 etc.).
- It provides two interfaces: I/O interface and Executor interface, which make Hyper can work with any socket and co-routine executor implementations.

![image](https://user-images.githubusercontent.com/6119088/207616551-6fdfddc7-c349-45f7-9cf9-42210633e7ae.png)

# Implementation

## Executor Interface
- In rt.rs, it defines a trait for execute():

		pub trait Executor<Fut> {
			fn execute(&self, fut: Fut);
		}
- The co-routine executor instance implements the trait to **get futures submitted from the Hyper library**.
For example, when Hyper users 

		pub struct TokioExecutor;
		impl<F> hyper::rt::Executor<F> for TokioExecutor
		where
			F: std::future::Future + Send + 'static,
			F::Output: Send + 'static,
		{
			fn execute(&self, fut: F) {
				tokio::task::spawn(fut);
			}
		}
